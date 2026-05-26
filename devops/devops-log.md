# DevOps Log

> Not a daily practice — logged whenever DevOps work is done in company time.
> Goal: track what I know and have implemented, build a picture of real DevOps experience.

---

## Skills Tracker

### CI/CD
- [x] GitHub Actions — workflow files, triggers, jobs
- [x] GitHub hosted runners (Ubuntu VM spun up by GitHub)
- [x] GitHub Secrets — storing SSH keys and sensitive values securely
- [x] SSH deployment — connecting to production server from pipeline
- [x] rsync — syncing build files from runner to server
- [ ] Self-hosted runners
- [ ] Rollback strategies
- [ ] Blue/green deployments

### Web Server / Proxy
- [x] Nginx — reverse proxy config, routing paths to ports
- [x] SSL/TLS — Let's Encrypt certificates, HTTPS setup
- [x] HTTP → HTTPS redirect
- [x] Subdomain routing (`*.bandyl.com`)
- [ ] Nginx load balancing config
- [ ] Rate limiting in Nginx

### Infrastructure
- [ ] Docker — containerizing services
- [ ] Docker Compose — multi-service local setup
- [ ] Kubernetes basics
- [ ] Cloud providers (AWS/GCP/Azure)

### Monitoring
- [ ] Server logs — reading access/error logs
- [ ] Uptime monitoring
- [ ] Alerting

---

## Implementation Log

### 2026-05-22 — CI/CD Pipeline for Frontend (HireMagic ATS iframe)
**What was built:**
- GitHub Actions pipeline on the frontend iframe repo
- Trigger: push to a specific branch → deploys to test environment
- Runner: GitHub hosted (Ubuntu VM spun up automatically)
- Steps:
  1. GitHub runner installs dependencies
  2. Builds frontend code (generates static files)
  3. Connects to production server via SSH (SSH key stored in GitHub Secrets)
  4. Uses `rsync` to copy build files into the iframe folder inside `hiremagic_ats_backend`

**Why rsync over simple copy:**
- rsync only transfers files that changed — faster, less bandwidth
- Atomic-ish — does not leave server in broken state mid-transfer

**Key concepts used:**
- GitHub Secrets — never put SSH keys or server IPs in code
- SSH key pair — private key in GitHub Secrets, public key on server's `authorized_keys`
- GitHub hosted runner — ephemeral VM, dies after pipeline finishes

---

---

## Networking Knowledge

### IP Addresses

**Two versions:**
- IPv4: `192.168.1.1` — 4 numbers (0–255), ~4 billion addresses, running out
- IPv6: `2001:0db8::8a2e:0370` — much longer, practically unlimited

**Public vs Private:**
| Type | What it is | Changes? |
|---|---|---|
| Public IP | Assigned by ISP, visible to internet | Changes when you switch networks |
| Private IP | Assigned by router, only inside local network | Changes when you switch networks |

**No permanent private IP** — it is assigned by whichever router you connect to. The only permanent hardware ID is the **MAC address** (burned into network card at factory).

**Private IP reserved ranges (never used on public internet):**
| Range | Common use |
|---|---|
| `192.168.x.x` | Home routers — most common |
| `10.x.x.x` | Office/enterprise networks |
| `172.16.x.x – 172.31.x.x` | Less common (WSL uses this) |

**Real example from my machine:**
- `192.168.1.45` — my laptop on home WiFi
- `192.168.1.1` — my router (Default Gateway)
- `172.31.16.1` — WSL virtual network
- `202.66.164.222` — my public IP (what internet sees)
- `103.212.121.59` — Bandyl's server public IP

**How NAT works (router translation):**
- Outgoing: router replaces `192.168.1.45` → `202.66.164.222` so internet can reply
- Incoming: router only forwards responses to requests you initiated — blocks everything else
- Result: your laptop is not directly accessible from the internet

**Full request journey:**
```
Laptop (192.168.1.45) → Router (192.168.1.1) → Internet (202.66.164.222) → Bandyl (103.212.121.59)
```

---

*(Add new entries below as DevOps work is done)*
