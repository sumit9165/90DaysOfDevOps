**visual cheat sheet / decision tree** — one page, easy to read under pressure.

Here’s a **text-based diagram**.

---

# 🗺️ ON-CALL CHEAT SHEET – DECISION TREE

```
                  [ Pager Alert / Incident ]
                             |
                             v
                  [ Check System Health ]
                             |
       ------------------------------------------------
       |                     |                       |
   CPU / Memory           Disk / IO             Network / DNS
       |                     |                       |
  top / free / ps       df -h / du -sh        ss / curl / dig
       |                     |                       |
High CPU?             Disk > 90%?            DNS failures?
  | Yes                  | Yes                  | Yes
  |                      |                      |
Kill / Restart        Truncate logs /         Restart resolver
Process / Pod         Investigate DB / Cache  / CoreDNS
Scale / Throttle      / Rotate logs
  |                      |
  |                      |
No?                     No?
  |                      |
  ------------------------
            |
       [ Service Logs ]
            |
    journalctl / tail / kubectl logs
            |
     -------------------
     |                 |
Errors / Warnings?   Normal?
     |                 |
Check upstream /     Monitor
DB / Cache / K8s      Continue
     |
     v
[ Containment Actions ]
  - nginx: reload / truncate logs
  - DB: terminate idle sessions / restart
  - Redis: increase maxmemory / LRU
  - K8s: scale / fix OOM / redeploy
  - Traffic spike: rate limit / caching
  - DNS: restart resolver / fallback server
            |
            v
[ If Incident Persists ]
  1. Deep debugging: strace, heap dumps, tcpdump
  2. Temporary mitigation: scale, disable debug logs
  3. Escalate: senior SRE / DBA / cloud provider
            |
            v
[ Update Runbook / Lessons Learned ]
```

---

### ✅ How to use this cheat sheet:

1. **Start at the top** — whatever triggered the pager.
2. **Follow the arrows** — diagnose resource → logs → containment → escalation.
3. **Quick actions are listed per node**, so you never waste time guessing.
4. **Add your service-specific notes** in each box for repeatable incidents.

---
