echo "Timestamp,% CPU (global),% CPU (user),% CPU (system)" > monitoreo_cpu.csv && \
top -b -d 10 -n 540 | awk '/%Cpu/ {
    "date +\"%Y-%m-%d %H:%M:%S\"" | getline t; 
    us=$2; sy=$4; id=$8; 
    gsub(",", ".", us); gsub(",", ".", sy); gsub(",", ".", id);
    print t "," 100-id "," us "," sy
}' >> monitoreo_cpu.csv
