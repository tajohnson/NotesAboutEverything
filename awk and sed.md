Add up sizes of emails from logging data

zcat ./30/amavis-20210930.gz  | grep "mr D" | awk '{print $(NF-1)}'  | awk '{ sum+=$1} END {print sum}'
