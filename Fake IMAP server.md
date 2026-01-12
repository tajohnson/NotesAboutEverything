
pip install twisted pyOpenSSL zope.interface service_identity


python3 fake_imap_server.py --imap-port 11030 --imaps-port 11032 --certfile /etc/letsencrypt/live/mailroute.net/fullchain.pem  --keyfile /etc/letsencrypt/live/mailroute.net/privkey.pem


python3 fake_imap_server.py --imap-port 11020 --imaps-port 11022 --certfile /etc/letsencrypt/live/mailroute.net/fullchain.pem  --keyfile /etc/letsencrypt/live/mailroute.net/privkey.pem


```
import argparse
from twisted.internet import reactor, protocol, ssl as twisted_ssl
from twisted.mail import imap4
from twisted.cred import portal, checkers, credentials
from twisted.protocols.policies import WrappingFactory


# Fake user account logic
class FakeIMAPUserAccount:
    def select(self, mailbox):
        raise imap4.IMAP4Exception("No mailboxes exist.")

    def listMailboxes(self, ref, wildcard):
        return []

    def getHierarchyDelimiter(self):
        return "/"

    def fetch(self, messages, uid):
        raise imap4.IMAP4Exception("Fetch not supported.")

    def search(self, query, uid):
        raise imap4.IMAP4Exception("Search not supported.")


# Realm and portal glue
class FakeIMAPRealm:
    def requestAvatar(self, avatarId, mind, *interfaces):
        return imap4.IAccount, FakeIMAPUserAccount(), lambda: None


class AlwaysAllowLogin:
    credentialInterfaces = (credentials.IUsernamePassword,)

    def requestAvatarId(self, creds):
        print(f"Accepted login: {creds.username.decode()} / {creds.password.decode()}")
        return creds.username


# IMAP server implementation
class FakeIMAPServer(imap4.IMAP4Server):
    def do_CAPABILITY(self, tag, rest):
        self.sendLine(b'* CAPABILITY IMAP4rev1 AUTH=PLAIN')
        self.sendLine(f'{tag} OK CAPABILITY completed'.encode())

    def do_LOGOUT(self, tag, rest):
        self.sendLine(b'* BYE Logging out')
        self.sendLine(f'{tag} OK LOGOUT completed'.encode())
        self.transport.loseConnection()

    def unknownCommand(self, tag, cmd, args):
        self.sendLine(f'{tag} NO Command "{cmd}" not supported'.encode())


# Factory wiring the protocol to the portal
class FakeIMAPFactory(protocol.Factory):
    def __init__(self):
        realm = FakeIMAPRealm()
        p = portal.Portal(realm)
        p.registerChecker(AlwaysAllowLogin())
        self.portal = p

    def buildProtocol(self, addr):
        return FakeIMAPServer(self.portal)


# Server startup
def start_servers(imap_port=1143, imaps_port=1993, certfile='server.crt', keyfile='server.key'):
    print(f"[+] Starting plaintext IMAP on port {imap_port}")
    reactor.listenTCP(imap_port, FakeIMAPFactory())

    print(f"[+] Starting SSL IMAPS on port {imaps_port}")
    context = twisted_ssl.DefaultOpenSSLContextFactory(keyfile, certfile)
    wrapped = WrappingFactory(FakeIMAPFactory())
    reactor.listenSSL(imaps_port, wrapped, context)

    print("[+] Server is running. Press Ctrl+C to stop.")
    reactor.run()


# CLI entrypoint
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description="Fake IMAP/IMAPS Server")
    parser.add_argument('--imap-port', type=int, default=1143, help='Port for plaintext IMAP')
    parser.add_argument('--imaps-port', type=int, default=1993, help='Port for IMAPS (SSL)')
    parser.add_argument('--certfile', default='/etc/letsencrypt/live/yourdomain.com/fullchain.pem', help='Path to SSL certificate')
    parser.add_argument('--keyfile', default='/etc/letsencrypt/live/yourdomain.com/privkey.pem', help='Path to SSL private key')
    args = parser.parse_args()

    start_servers(args.imap_port, args.imaps_port, args.certfile, args.keyfile)

```