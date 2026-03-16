LC_ALL=C top -b -d 5 -n 1080 | awk '/top -/ {printf $3} /%Cpu/ {print ";" $2 ";" $4 ";" $2+$4}' | tr '.' ',' > monitoreo.csv



#!/bin/bash

ARCHIVO="monitor_paralelo.csv"
echo "Timestamp;% CPU (global);% Memoria Principal" > $ARCHIVO

LC_ALL=C top -b -d 5 -n 1440 | awk '
  /top -/ {hora=$3}
  /%Cpu/ {cpu=$2+$4}
  /Mem/ && !/Swap/ {
    mem=($8/$4)*100
    printf "%s;%.1f;%.1f\n", hora, cpu, mem
  }
' | tr '.' ',' >> $ARCHIVO
