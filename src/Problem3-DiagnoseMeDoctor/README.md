## Troubleshooting for High Memory Usage on NGINX VM
### 1. Verify Memory Usage via CLI
Command: `free -h` or `htop`
- Check available memory to confirm actual usage. Linux caches disk I/O (buff/cache), which may appear as "used" memory but is reclaimable.
- If available is low (e.g., <1GB), proceed to investigate.

### 2. Identify Processes Consuming Memory
Command: `top` or `ps aux --sort=-%mem`
- Look for NGINX worker processes or unrelated services. Example: If nginx workers use 1GB each and 50 workers exist, this explains high usage.

### 3. Check NGINX Configuration
File: `/etc/nginx/nginx.conf`
- Key Parameters:
    - `worker_processes auto;` → Might spawn too many workers (e.g., equal to CPU cores).
    - `worker_connections 1024;` → High connections per worker increase memory.
    - `Buffer settings` (e.g., `proxy_buffer_size`, `proxy_buffers`) → Oversized buffers waste memory.
- Fix: Set `worker_processes` to a fixed number (e.g., 2-4 for a load balancer) and tune buffers.

### 4. Check for Memory Leaks
NGINX Logs: `/var/log/nginx/error.log`
- Look for repeated allocation failures or module errors (e.g., third-party modules like Lua).
- System Logs: `journalctl -u nginx or /var/log/syslog`
- Check for OOM (Out-of-Memory) killer activity: `grep -i kill /var/log/syslog.`

### 5. Investigate Non-NGINX Processes
Commands:
- `sudo ss -tulpn` → Identify unexpected open ports/processes.
- `sudo apt list --installed` → Check for unauthorized packages.
- `crontab -l and /etc/cron.d/` → Look for suspicious cron jobs.

### 6. Check Kernel and System Health
Command: `dmesg | grep -i memory`
- Look for kernel-level memory allocation errors.

Action: If a kernel bug is suspected, update Ubuntu: `sudo apt update && sudo apt upgrade -y`

## Possible Root Causes, Impacts, and Recovery
### 1. NGINX Misconfiguration
- **Cause**: Excessive workers (`worker_processes`), oversized buffers, or high `worker_connections`.
- **Impact**: Slow response times, NGINX crashes, or OOM kills.
- **Recovery**:
    - Reduce `worker_processes` to match CPU cores.
    - Lower buffer sizes and `worker_connections`.
    - Reload NGINX: `sudo systemctl reload nginx`.

### 2. Memory Leak in NGINX Module
- **Cause**: Faulty third-party module (e.g., custom Lua script).
- **Impact**: Gradual memory exhaustion, service downtime.
- **Recovery**:
    - Disable the module in `nginx.conf`.
    - Restart NGINX: `sudo systemctl restart nginx`.
    - Update or replace the module.

### 3. Unrelated Process Consuming Memory
- **Cause**: Malware, backup jobs, or unintended services (e.g., Apache).
- **Impact**: Resource contention, security risks.
- **Recovery**:
    - Terminate the process: `sudo kill -9 <PID>`.
    - Remove unauthorized software: `sudo apt purge <package>`.
    - Audit startup processes: `systemctl list-units --type=service`.

### 4. Kernel Memory Leak
- **Cause**: Kernel bug (e.g., slab memory leak).
- **Impact**: System instability, crashes.
- **Recovery**:
    - Reboot the VM.
    - Apply kernel updates and monitor.

### 5. Misleading Monitoring Metrics
- **Cause**: Monitoring tool misinterprets `buff/cache` as "used" memory.
- **Impact**: False alerts.
- **Recovery**: Configure monitoring to track `available` memory instead of `used`.