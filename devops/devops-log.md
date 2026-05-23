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

*(Add new entries below as DevOps work is done)*
