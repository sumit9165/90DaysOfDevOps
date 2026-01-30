
# PHASE 1 — 🔥 Simulated Failure Drill (CPU + Disk)

### 🚨 Alert

> **High CPU usage detected. SSH logins slow.**

---

## Step 1: CPU spike investigation

```bash
top
```

**Simulated output (snippet):**

```
PID   USER   %CPU  %MEM  COMMAND
2143  root   185.3  0.2   yes
892   root     0.1  0.1   sshd
```

**Interpretation:**
A runaway `yes` process is consuming >180% CPU (multi-core). SSH is slow because scheduler is busy.

---

## Step 2: Confirm process details

```bash
ps -o pid,ppid,pcpu,pmem,etime,comm -p 2143
```

**Observed:**

* Long-running
* No parent process of interest
* Not a legit system service

---

## Step 3: Disk pressure appears

```bash
df -h
```

**Simulated output:**

```
/dev/sda1   40G   39G   1G  98% /
```

```bash
du -sh /var/log/*
```

**Observed:**
`/var/log/auth.log` = **12GB**

👉 CPU + disk pressure = compound incident

---

## Immediate containment

```bash
kill -9 2143
```

```bash
truncate -s 0 /var/log/auth.log
```

✅ System stabilizes
⚠️ Root cause still unknown

---

# PHASE 2 — 🐳 Docker / App-Focused Runbook

Now assume the **real culprit** was a container.

## Step 1: Container health

```bash
docker ps
```

**Simulated output:**

```
CONTAINER ID   NAME        STATUS
f3a9c1         auth-api    Up 3 days
```

```bash
docker stats --no-stream
```

```
auth-api   CPU 160%   MEM 512MB / 1GB
```

**Interpretation:**
Container is CPU-bound and likely spamming logs → disk pressure.

---

## Step 2: Inspect container logs

```bash
docker logs --tail 20 auth-api
```

**Observed:**
Repeated authentication failures + stack trace loop.

---

## Step 3: App-level fix

```bash
docker restart auth-api
```

CPU drops back to normal.

---

# PHASE 3 — 🧠 Runbook → Incident Playbook Upgrade

Here’s how your runbook now evolves 👇

### Severity Classification

| Severity | Description                             |
| -------- | --------------------------------------- |
| SEV-1    | SSH inaccessible, CPU >90%, disk >95%   |
| SEV-2    | Performance degraded, service reachable |
| SEV-3    | Errors in logs, no user impact          |

---

### Decision Tree (muscle memory)

```
Alert fires
 ├─ CPU high?
 │   ├─ yes → top → identify PID → kill or restart service
 │   └─ no
 ├─ Disk >90%?
 │   ├─ yes → du → logs → rotate/truncate
 │   └─ no
 └─ Service down?
     ├─ yes → systemctl/docker restart
     └─ escalate
```

---

### Escalation Triggers

* Restart fails twice
* Disk refills within 10 minutes
* CPU spike returns after fix

---

# PHASE 4 — 🧪 Timed On-Call Simulation (You’re On Call)

⏱️ **Timer starts now**

🚨 **Pager Alert**

> SSH connections intermittently failing
> CPU spikes every 5 minutes
> Disk usage climbing

---

## 🚨 ON-CALL INCIDENT — LIVE SIMULATION CONTINUES

### Commands executed

```bash
top
df -h
ps aux --sort=-%cpu | head
```

---

## 🔍 Command 1: `top`

```
%Cpu(s): 92.1 us,  6.3 sy,  0.0 id
PID   USER   %CPU  %MEM  COMMAND
2143  root   187.4  0.2   auth-api
892   root     0.3  0.1   sshd
```

**Interpretation**

* CPU is nearly saturated
* `auth-api` process (likely container) is the culprit
* sshd itself is not misbehaving, just impacted

---

## 💽 Command 2: `df -h`

```
Filesystem      Size  Used Avail Use%
/dev/sda1        40G   39G  1.0G   98% /
```

**Interpretation**

* Disk is critically full
* SSH failures likely due to log writes + CPU contention
* This is now **SEV-1**

---

## 🔥 Command 3: `ps aux --sort=-%cpu | head`

```
USER   PID  %CPU  %MEM  COMMAND
root  2143  187.4  0.2  auth-api
root  2190   22.1  0.1  dockerd
```

**Interpretation**

* Confirms runaway app
* Docker daemon under stress
* Need **containment NOW**

---

## 🛑 Immediate containment (what you should do next)

You now have **two correct paths**:

### Option A — Kill the offender fast

```bash
kill -9 2143
```

### Option B — Control blast radius (preferred)

```bash
docker restart auth-api
```

---






















