
 zgrep abuse ./*/policyd_smtpauth* | awk '{print $NF}' | sort | uniq -c