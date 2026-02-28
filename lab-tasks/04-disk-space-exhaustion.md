# Disk Space Exhaustion Simulation

## Objective
To simulate a disk space exhaustion incident, investigate potential causes, identify the root cause, and restore system storage using Linux monitoring and troubleshooting commands.

---

## Baseline Disk Check

### Command Executed
df -h

### Output Observed
- Filesystem: /dev/mapper/ubuntu--vg-ubuntu--lv mounted on `/`
- Total Size: 12G
- Used: 4.9G
- Available: 5.8G
- Usage: 46%

### Interpretation
Disk usage was within normal operating limits.

---

## Disk Consumption Simulation

### Commands Executed
sudo fallocate -l 1G largefile1.img  
sudo fallocate -l 1G largefile2.img  
sudo fallocate -l 1G largefile3.img  

### Output Observed
- Disk usage increased gradually
- Final usage reached approximately 93%

### Interpretation
This simulated a critical disk alert scenario caused by rapid storage consumption.

---

## Alert Validation

### Command Executed
df -h

### Output Observed
- Disk usage at 93%

### Interpretation
Disk utilization crossed critical threshold levels and required investigation.

---

## Initial Investigation

### Command Executed
ls -lh /

### Output Observed
- Observed `swap.img` consuming approximately 2.3G

### Interpretation
Although `swap.img` appeared large, it is a system-managed swap file and not the cause of sudden disk growth.

This step highlights the importance of validating system files before taking corrective action.

---

## Directory Verification

### Command Executed
pwd

### Output Observed
- Confirmed current working directory was the user home directory

### Command Executed
ls -lh

### Output Observed
- Identified multiple `largefile*.img` files
- Each file approximately 1GB

### Interpretation
The disk usage spike was caused by large user-created files consuming storage within the home directory.

---

## Controlled Incident Resolution

### Command Executed
rm largefile1.img

### Output Observed
- Disk usage reduced from 93% to 84%

### Interpretation
Initial cleanup confirmed that the identified files were the root cause.

---

## Complete Cleanup

### Command Executed
rm largefile*.img

### Output Observed
- Disk usage returned to baseline levels

---

## Post-Incident Validation

### Command Executed
df -h

### Output Observed
- Used: 4.9G
- Available: 5.8G
- Usage: 46%

### Interpretation
System storage was fully restored, confirming successful incident resolution.

---

## Skills Practiced

- Disk monitoring using `df`
- File inspection using `ls`
- Directory verification using `pwd`
- Identifying misleading system files
- Controlled cleanup procedures
- Post-incident validation workflow

---

## Conclusion

This exercise simulated a real-world disk exhaustion incident. The investigation required distinguishing between system-managed files and abnormal user-generated storage growth, followed by controlled cleanup and validation of system recovery.
