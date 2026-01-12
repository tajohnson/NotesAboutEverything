User: mr-es-snapshot
	Access Key: <See "Secure info">
	Secret Key:  <See "Secure info">
	
	
On ES 7.6 - on each node, do this, and then restart the node:

# ES_PATH_CONF=/etc/elasticsearch/loghost /usr/share/elasticsearch/bin/elasticsearch-plugin install repository-s3

Then do this:

ES_PATH_CONF=/etc/elasticsearch/loghost /usr/share/elasticsearch/bin/elasticsearch-keystore add s3.client.default.access_key

ES_PATH_CONF=/etc/elasticsearch/loghost /usr/share/elasticsearch/bin/elasticsearch-keystore add s3.client.default.secret_key

# a session token is optional so the following command may not be needed
#bin/elasticsearch-keystore add s3.client.default.session_token

Now restart each node.

First delay rebuild so it gives time for restart:
curl --user <password>  -X PUT "localhost:9200/_all/_settings?pretty" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
  }
}
'



curl --user <password>  -X PUT "localhost:9200/_snapshot/mr-s3-repository?pretty" -H 'Content-Type: application/json' -d'
{
  "type": "s3",
  "settings": {
    "bucket": "mr-es-snapshot-lax"
  }
}
'




