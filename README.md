LC_ALL=C top -b -d 5 -n 1080 | awk '/top -/ {printf $3} /%Cpu/ {print ";" $2 ";" $4 ";" $2+$4}' | tr '.' ',' > monitoreo.csv
