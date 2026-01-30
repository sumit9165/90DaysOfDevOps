* You chose **maximum SRE mode**.
* Let’s do **all three for `nginx`**, clean and reusable:

* 1️⃣ **Simulate breaking nginx on purpose**
* 2️⃣ **Troubleshoot it step-by-step**
* 3️⃣ **End with a reusable on-call runbook template**

You can literally reuse this in real incidents or interviews.

---

# 🔥 NGINX FAILURE DRILL (SIMULATED)

## Target service

* **Service:** nginx
* **Role:** Web server / reverse proxy
* **Impact:** User-facing traffic

---

## 🚨 Pager Alert

* > Website returning 502 errors
* > Requests timing out intermittently
* > CPU usage spiking

---

## Step 1 — Health snapshot (muscle memory)

```bash
systemctl status nginx
```

**Output**

```
nginx.service - A high performance web server
Active: active (running)
```

🧠 *Service is “up” but may be unhealthy.*

---

```bash
top
```

```
PID   USER   %CPU  %MEM  COMMAND
3112  www    165.2  1.4   nginx
```

🧠 *Worker process burning CPU.*

---

```bash
df -h
```

```
/dev/sda1   40G   38G   2G  95% /
```

🧠 *Disk pressure approaching critical.*

---

## Step 2 — Network validation

```bash
ss -tulpn | grep nginx
```

```
LISTEN 0 511 0.0.0.0:80 users:(("nginx",pid=3112))
```

🧠 *Port is listening.*

---

```bash
curl -I http://localhost
```

```
HTTP/1.1 502 Bad Gateway
```

🧠 *Frontend up, backend failing.*

---

## Step 3 — Logs (where truth lives)

```bash
tail -n 50 /var/log/nginx/error.log
```

```
connect() failed (111: Connection refused) while connecting to upstream
```

🧠 *Upstream app is dead or unreachable.*

---

```bash
du -sh /var/log/nginx
```

```
14G /var/log/nginx
```

🧠 *Log spam explains disk pressure.*

---

## Step 4 — Root cause found 😈

Someone **accidentally enabled debug logging**:

```nginx
error_log /var/log/nginx/error.log debug;
```

This causes:

* Massive log volume
* Disk pressure
* CPU spike
* 502s under load

---

## Step 5 — Containment & fix

```bash
sed -i 's/debug;/warn;/' /etc/nginx/nginx.conf
```

```bash
nginx -t
systemctl reload nginx
```

```bash
truncate -s 0 /var/log/nginx/error.log
```

✅ CPU drops
✅ Disk stabilizes
✅ 502s disappear

---

# 🧠 QUICK FINDINGS

* nginx process healthy but overloaded
* Upstream errors amplified by debug logging
* Disk nearly full due to runaway logs
* Reload (not restart) sufficient

---

# 🧯 IF THIS WORSENS (NEXT STEPS)

1. Temporarily disable access logs

   ```bash
   access_log off;
   ```

2. Rate-limit requests

   ```nginx
   limit_req zone=one burst=10 nodelay;
   ```

3. Capture traffic

   ```bash
   tcpdump -i eth0 port 80
   ```

---

# 📘 REUSABLE ON-CALL RUNBOOK TEMPLATE 

```
#NGINX On-Call Runbook

##Service Overview
- Service name:
- Ports:
- Upstream dependencies:

## Immediate Checks (Run in Order)
1. systemctl status nginx
2. top / ps --sort=-%cpu
3. df -h
4. ss -tulpn | grep nginx
5. curl -I localhost

## Logs
- tail -n 50 /var/log/nginx/error.log
- du -sh /var/log/nginx

## Common Failure Patterns
- 502 errors → upstream down
- High CPU → log level, request flood
- Disk full → access/error logs

## Safe Actions
- nginx -t
- systemctl reload nginx
- truncate logs (never rm)

## Escalation Triggers
- Reload fails
- Disk refills in <10 minutes
- Errors persist after upstream restart

## Notes / Learnings
- (fill during incident)
```

---

## 🎯 What you just practiced

* ✔ Real nginx failure
* ✔ CPU + disk + logs correlation
* ✔ Safe reload vs restart
* ✔ Interview-grade runbook
* ✔ True on-call thinking

---


