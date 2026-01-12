

1. [x] Update to MySQL 8 on aurora
	1. [x] Testing tj
	2. [x] Testing vlad
	3. [ ] tj - move
2. smtpauth - new dovecot sasl that supports both passwords
	1. [x] 001.lax testing new dovecot which supports both
		1. [ ] do ansible-playbook ./playbooks/filters.yml --tags mr_auth_server,dovecot-sasl,postfix-smtpauth for both 008.lax and 009.lax
3. [x] Add autoresponder to OLD custom hook
	1. [ ] on 002.lax /etc/amavisd/amaivsd.mroute.custom_autoresponder.conf - needs to be tested using dev with new code
4. [x] Release Hosted Mail, plans, roles
5. [ ] Upgrade v2.0c to use feature_settings table
6. [ ] Move to new custom hook?
7. [ ] Move settings to feature_settings table, new custom hook
8. [ ] New WBList interface
9. [ ] Mangler

With this sort of plan, I can go in and upgrade v2.0c to new feature settings, start work on mangler, etc.

Idea: 
- [ ] backport autoresponder to old hook.
- [ ]  update v2.0c with support for feature_settings table
1. [ ] Document every change made to standard tables for this for vlad
2. [ ] then start on mangler code


Idea for Mangler:
	- Could be a perl module
	- Could do interface to standalone server daemon  ( perl? python? Go? )


