# Port Conflict & Service Binding Failure

## Objective

Simulate a scenario where a service fails to start because another process is already using the required port.

---

# Baseline Verification

Command executed:

sudo systemctl status nginx

Output observed:

Active: active (running)

HTTP validation:

curl -I localhost

Result:

HTTP/1.1 200 OK  
Server: nginx

---

# Simulated Port Conflict

Stopped nginx to free port 80:

sudo systemctl stop nginx

Started a temporary Python HTTP server using port 80:

sudo python3 -m http.server 80

---

# Service Startup Failure

Attempted to start nginx:

sudo systemctl start nginx

Result:

Job for nginx.service failed because the control process exited with error code.

Service status:

Active: failed

Error message observed:

bind() to 0.0.0.0:80 failed (98: Address already in use)

---

# Root Cause Investigation

Checked which process was using port 80:

sudo ss -tulpn | grep :80

Output observed:

python3 LISTEN 0.0.0.0:80

Interpretation:

The Python HTTP server was already using port 80, preventing nginx from binding to the port.

---

# Resolution

Stopped the conflicting Python service.

Restarted nginx:

sudo systemctl start nginx

---

# Validation

Verified service status:

sudo systemctl status nginx

Result:

Active: active (running)

Confirmed HTTP response:

curl -I localhost

Output:

HTTP/1.1 200 OK  
Server: nginx

---

# Skills Practiced

- Investigating port conflicts
- Identifying processes using network ports
- Troubleshooting service startup failures
- Using `ss` to inspect listening ports
- Resolving service binding issues
- Validating application availability

---

# Conclusion

This exercise simulated a real-world incident where a service failed to start due to a port conflict. By identifying the process occupying the port and stopping it, the nginx service was successfully restored.
