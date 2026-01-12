grep -v "TIMING-SA" ./amavis-timing-20121211  |  awk 'BEGIN{s=0;}{s=s+$9;}END{print s/NR;}'

