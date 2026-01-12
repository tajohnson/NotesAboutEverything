
Need to add a pattern for these:

2019-05-16T02:13:51+0000 001.mia postfix-inbound/smtp[3058]: SSL_connect error to 206.214.242.90[206.214.242.90]:25: Connection reset by peer
	
	



    -
#      paths:
#        - /var/log/20*/*/*/policyd*
#      input_type: log
#      document_type:
#      tail_files: true
#      exclude_files: [".gz$"]
#    -
#      paths:
#        - /var/log/20*/*/*/postfix-gw*
#      input_type: log
#      document_type:
#      tail_files: true
#      exclude_files: [".gz$"]
#   

