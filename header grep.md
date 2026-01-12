# header grep

awk '
  BEGIN {RS=""; FS="\n"}
  /Authentication-Results: mailroute.net;/ {
    found = 0
    for (i=1; i<=NF; i++) {
      if ($i ~ /^Authentication-Results: mailroute.net;/) {
        found = 1
        print $i
      } else if (found && $i ~ /^[[:space:]]+/) {
        print $i
      } else if (found && $i !~ /^[[:space:]]+/) {
        break
      }
    }
  }
' mbox




