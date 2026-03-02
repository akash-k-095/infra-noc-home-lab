# Memory Pressure Simulation

## Objective
To simulate high memory usage, observe system behavior under memory pressure, identify the memory-consuming process, and validate system recovery after the stress condition ends.

---

## Baseline Memory Check

### Commands Executed
free -h  
top  

### Output Observed
- Total Memory: 3.6 GiB  
- Used: ~418 MiB  
- Free: ~3.3 GiB  
- Available: ~3.4 GiB  
- Buff/cache: ~285 MiB  
- Swap: 2.2 GiB total, 0B used  

### Interpretation
The system memory was in a healthy state with very low usage and no swap activity.

---

## Memory Stress Simulation

### Command Executed
stress --vm 1 --vm-bytes 2G --timeout 60

### Observed Behavior
- Virtual machine became noticeably slow
- Commands responded slowly
- System performance temporarily degraded

### Interpretation
The stress process allocated approximately 2 GB of RAM, creating memory pressure. The system experienced reduced responsiveness as the kernel prioritized memory management.

---

## Post-Stress Validation

### Command Executed
free -h

### Output Observed
- Used Memory: ~580 MiB  
- Buff/cache increased to ~324 MiB  
- Swap usage remained at 0B  

### Interpretation
Memory usage returned near baseline after the stress process terminated. Increased buff/cache indicates Linux memory optimization and does not represent a memory leak.

---

## Process-Level Memory Investigation

### Commands Executed
stress --vm 1 --vm-bytes 1G --timeout 60 &  
top  

### Output Observed
- Stress process identified under PID 1193  
- VIRT memory: ~1 GB  
- RES memory: ~247 MB  
- State: Running  
- CPU usage: ~100%  
- Memory usage: ~6%  

### Interpretation
The stress process was confirmed as the primary consumer of system memory. The VIRT value represented total allocated virtual memory, while RES showed actual physical memory usage.

---

## Stress Process Completion

### Output Observed
- "stress: info: [1192] successful run completed in 61s"

### Interpretation
The memory stress process completed successfully within the configured timeout period. No forced termination or out-of-memory (OOM) events occurred. System stability was restored automatically after the process exited.

---

## Skills Practiced

- Monitoring memory usage using `free` and `top`
- Understanding Linux memory allocation behavior
- Identifying memory-intensive processes
- Interpreting VIRT vs RES memory metrics
- Observing system behavior under memory pressure
- Validating system recovery after high memory usage

---

## Conclusion

This simulation demonstrated how high memory allocation affects system performance. The investigation confirmed the stress process as the root cause, and the system recovered automatically after process completion without swap usage or OOM events.
