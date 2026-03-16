LC_ALL=C top -b -d 5 -n 1080 | awk '/top -/ {printf $3} /%Cpu/ {print ";" $2 ";" $4 ";" $2+$4}' | tr '.' ',' > monitoreo.csv






#!/bin/bash

# Archivo de salida para la prueba
ARCHIVO="prueba_10s.csv"

# 1. Escribimos la cabecera
echo "Timestamp;% CPU (global);% Memoria Principal" > $ARCHIVO

echo "Iniciando prueba rápida de 10 segundos..."

# 2. Monitor TOP configurado para 10 segundos (-d 1 -n 10)
LC_ALL=C top -b -d 1 -n 10 | awk '
  /top -/ {hora=$3}
  /%Cpu/ {cpu=$2+$4}
  /Mem/ {
    # Calculamos el porcentaje de memoria usada sobre la total
    mem=($8/$4)*100
    printf "%s;%.1f;%.1f\n", hora, cpu, mem
  }
' | tr '.' ',' >> $ARCHIVO

echo "¡Prueba terminada! Abre el archivo $ARCHIVO en Calc para comprobarlo."
