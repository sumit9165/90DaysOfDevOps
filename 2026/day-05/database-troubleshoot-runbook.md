# 💥 DATABASE FAILURE DRILL (Postgres)

*(Same logic applies to MySQL — I’ll note the equivalents)*

## Target service

* **Service:** PostgreSQL
* **Impact:** App returns 500s / timeouts

---

## 🚨 Pager Alert

> API error rate >30%
> Requests timing out
> DB connections exhausted

---

## Step 1 — Immediate health snapshot

```bash
systemctl status postgresql
```

**Observed:** Service active (running)
🧠 *Service “up” doesn’t mean healthy.*

---

```bash
top
```

```
PID   USER   %CPU  %MEM  COMMAND
1421  postgres  95.3  42.1  postgres
```

🧠 *CPU-bound database process.*

---

```bash
free -h
```

**Observed:** Memory tight, cache growing, no swap yet.

---

## Step 2 — Disk & IO (DB killer)

```bash
df -h
```

```
/dev/sda1   40G   39G   1G  98% /
```

🧠 *Databases + full disk = incoming outage.*

---

```bash
iostat -xz 1 3
```

**Observed:**

* High `await`
* Disk utilization >90%

---

## Step 3 — Database-level checks

```bash
sudo -u postgres psql -c "SELECT count(*) FROM pg_stat_activity;"
```

```
count
-----
300
```

```bash
sudo -u postgres psql -c "SHOW max_connections;"
```

```
100
```

🧠 **ROOT CAUSE FOUND**

* Connection exhaustion
* App leaking DB connections

(MySQL equivalent: `SHOW PROCESSLIST;`)

---

## Step 4 — Containment (NOW)

```bash
sudo -u postgres psql -c "
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE state = 'idle';
"
```

```bash
systemctl restart postgresql
```

* ✅ Connections drop
* ✅ CPU stabilizes
⚠️ Root cause still app-side

---

## If this worsens (DB)

* 1. Enable slow query logging
* 2. Lower `max_connections`, add pooling (PgBouncer)
* 3. Capture query stats:

   ```sql
   SELECT * FROM pg_stat_statements;
   ```

---

# 🔁 TRAFFIC SPIKE + RATE LIMITING LAB (nginx)

## 🚨 Scenario

* > Traffic spikes 10×
* > Backend healthy
* > nginx starts failing

---

## Step 1 — Confirm spike

```bash
ss -s
```

**Observed:** Surge in ESTABLISHED connections

```bash
nginx_status
```

**Observed:** Active connections skyrocketing

---

## Step 2 — Protect the backend (rate limit)

```nginx
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;

server {
  location /api/ {
    limit_req zone=api_limit burst=20 nodelay;
    proxy_pass http://backend;
  }
}
```

```bash
nginx -t && systemctl reload nginx
```

---

## Step 3 — Verify protection

```bash
curl -I http://localhost/api
```

```
HTTP/1.1 429 Too Many Requests
```

🧠 *nginx sheds load before backend dies.*

---

## If this worsens (traffic)

* 1. Add caching
* 2. Enable connection limits
* 3. Scale horizontally

---

# 🎤 MOCK SRE INTERVIEW (YOU’RE ON STAGE)

I’ll be the interviewer.
You answer **out loud or in text**. No perfect answers needed — clarity > buzzwords.

---

### Question 1

> You’re paged for elevated API latency. Where do you start and why?

---

### Question 2

> PostgreSQL is “running” but the app is down. How do you prove it’s a DB issue?

---

### Question 3

> Why is restarting the database dangerous? When is it acceptable?

---

### Question 4

> How does rate limiting protect downstream services?

---

### Question 5 (Senior-level)

> What signals tell you this is an **app bug**, not an infrastructure failure?

---

### Bonus

> What would you automate after surviving this incident?

---

## 🧠 What you’ve now practiced

* ✅ DB outage under load
* ✅ Connection exhaustion
* ✅ Disk + IO correlation
* ✅ Traffic spike defense
* ✅ Real SRE interview reasoning
* ✅ Runbook-driven thinking (not guessing)

---
