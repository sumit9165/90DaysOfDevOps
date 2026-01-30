
# 💣 KUBERNETES POD CRASHLOOP + OOM KILL

## Target service

**Service:** `web-app` deployment in Kubernetes
**Impact:** Pod crashlooping, high memory usage

---

## 🚨 Pager Alert

> Deployment has pods in `CrashLoopBackOff`
> Users report intermittent 500s

---

## Step 1 — Inspect pods

```bash
kubectl get pods -n production
```

```
NAME                     READY   STATUS             RESTARTS   AGE
web-app-6d7f89f6b7-hj9k2 0/1     CrashLoopBackOff   5          12m
```

---

## Step 2 — Check pod logs

```bash
kubectl logs web-app-6d7f89f6b7-hj9k2 -n production
```

```
RuntimeError: Memory allocation failed
```

**Observed:** Pod OOMing → crashloop

---

## Step 3 — Describe pod for Kubernetes events

```bash
kubectl describe pod web-app-6d7f89f6b7-hj9k2 -n production
```

```
Events:
  Type     Reason       Age   From               Message
  ----     ------       ----  ----               -------
  Warning  OOMKilled    5m    kubelet            Container killed due to memory usage
```

**Root cause:** Container exceeded memory limit

---

## Step 4 — Containment

* Temporarily scale deployment to reduce pressure:

```bash
kubectl scale deployment web-app --replicas=0 -n production
```

* Adjust memory limits and requests in the manifest:

```yaml
resources:
  requests:
    memory: "512Mi"
  limits:
    memory: "1024Mi"
```

* Redeploy:

```bash
kubectl apply -f web-app-deployment.yaml
kubectl scale deployment web-app --replicas=3 -n production
```

✅ Pod stabilizes, no CrashLoopBackOff

---

## If this worsens (next steps)

1. Enable heap dumps for debugging
2. Use `kubectl exec` to inspect memory usage inside container
3. Add horizontal pod autoscaling to handle spikes

---

# 🧠 CACHING FAILURE (Redis meltdown)

## Pager Alert

> Cache latency spikes → DB load increases
> App response slows, timeouts occur

---

## Step 1 — Check Redis status

```bash
redis-cli ping
```

```
PONG
```

✅ Redis alive, but may be overloaded

---

## Step 2 — Inspect memory

```bash
redis-cli info memory
```

```
used_memory: 1050000000
maxmemory: 1073741824
```

**Observed:** ~1GB used out of 1GB → hitting maxmemory

---

## Step 3 — Eviction / slow commands

```bash
redis-cli info stats
```

```
evicted_keys: 1200
expired_keys: 1500
```

**Interpretation:** Keys being evicted → app cache misses → DB pressure

---

## Step 4 — Containment

* Temporarily increase `maxmemory`:

```bash
redis-cli config set maxmemory 2gb
```

* Use LRU eviction policy:

```bash
redis-cli config set maxmemory-policy allkeys-lru
```

✅ Cache stabilizes, DB pressure drops

---

## Next steps if worsens

1. Add Redis clustering / sharding
2. Enable monitoring + alerts on `used_memory`
3. Profile app caching pattern → prevent cache storm

---

# 🌐 DNS OUTAGE SIMULATION

## Pager Alert

> App cannot resolve backend hostnames → failing requests

---

## Step 1 — Test DNS resolution

```bash
dig backend.service.local
```

```
;; connection timed out; no servers could be reached
```

✅ DNS failure confirmed

---

## Step 2 — Check local resolver

```bash
cat /etc/resolv.conf
```

```
nameserver 10.0.0.2
```

```bash
ping 10.0.0.2
```

✅ Resolver unreachable

---

## Step 3 — Containment

* Restart local DNS service / kube-dns:

```bash
systemctl restart systemd-resolved
kubectl rollout restart deployment coredns -n kube-system
```

✅ Resolution restored, app connectivity returns

---

## If this worsens

1. Switch to secondary resolver (`8.8.8.8`) temporarily
2. Use static `/etc/hosts` overrides for critical services
3. Investigate upstream DNS provider

---

# 🎯 LIVE WHITEBOARD INTERVIEW — “DESIGN A RESILIENT SYSTEM”

**Prompt:** Design a resilient multi-tier web system.

**Model Answer / Thought Process:**

1. **Frontend layer**

   * Load balancer (HAProxy / Nginx / ALB)
   * Auto-scaling web servers
   * Health checks + rate limiting

2. **Application layer**

   * Stateless containers or VMs
   * Horizontal scaling
   * Graceful shutdown + rolling updates

3. **Database layer**

   * Primary + read replicas
   * Connection pooling
   * Backups + failover

4. **Caching layer**

   * Redis / Memcached cluster
   * LRU eviction, persistent failover

5. **Observability**

   * Metrics (Prometheus, Grafana)
   * Logging + alerting
   * Distributed tracing (Jaeger / OpenTelemetry)

6. **Failure scenarios considered**

   * Node / pod crash → HPA / replication
   * Disk full → monitoring + retention policies
   * Network partition → retries, circuit breakers
   * DNS / service discovery → multiple resolvers

7. **Automation / IaC**

   * Terraform / Helm / Ansible to version configs
   * CI/CD pipelines with canary rollout
   * Runbooks for common failures

✅ Key: explain reasoning **step by step**, not just draw boxes

---

