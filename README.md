LC_ALL=C top -b -d 5 -n 1080 | awk '/top -/ {printf $3} /%Cpu/ {print ";" $2 ";" $4 ";" $2+$4}' | tr '.' ',' > monitoreo.csv




#!/bin/bash

ARCHIVO="prueba_10s.csv"
echo "Timestamp;% CPU (global);% Memoria Principal" > $ARCHIVO
echo "Iniciando prueba rápida de 10 segundos..."

LC_ALL=C top -b -d 1 -n 10 | awk '
  /top -/ {hora=$3}
  /%Cpu/ {cpu=$2+$4}
  /[Mm]em/ && !/Swap/ {
    for(i=1; i<=NF; i++) {
      if($i ~ /total/) total=$(i-1)
      if($i ~ /used/) used=$(i-1)
    }
    gsub(/[^0-9.]/, "", total)
    gsub(/[^0-9.]/, "", used)
    if (total > 0) {
      mem = (used / total) * 100
    } else {
      mem = 0
    }
    printf "%s;%.1f;%.1f\n", hora, cpu, mem
  }
' | tr '.' ',' >> $ARCHIVO

echo "¡Prueba terminada! Abre el archivo $ARCHIVO en Calc."
