# DevSecOps Roadmap — Tracker

*Medium → Hero. No timeframes — move on when the current thing is boring, not when a calendar says so. Every box you tick means you actually did the task.*

---

## 🗺️ Overall Progress

- [ ] **Stage 0** — Workbench set up
- [ ] **Stage 1A** — Linux
- [ ] **Stage 1B** — Bash
- [ ] **Stage 1C** — Networking
- [ ] **Stage 1D** — Git
- [ ] **🏆 Stage 1 Capstone** — Router + Sensor + SIEM + auto-response
- [ ] **Stage 2A** — Containers & Docker
- [ ] **Stage 2B** — Cloud (AWS or Azure)
- [ ] **Stage 2C** — Terraform
- [ ] **Stage 2D** — Kubernetes
- [ ] **Stage 2E** — CI/CD & System Design
- [ ] **🚀 Stage 2 Flagship** — The self-defending GitOps platform
- [ ] **Stage 3** — Real-world cybersec projects (pick 2–3)

---

## STAGE 0 — Set Up the Workbench

### EC2 setup
- [ ] AWS account created, MFA on root, billing alarm set first
- [ ] Ubuntu Server LTS `t3.micro` launched (free-tier checked)
- [ ] Key pair created, `.pem` chmod'd to 400
- [ ] Security group: SSH from your IP only
- [ ] SSH in successfully
- [ ] AWS CLI installed locally; `aws sts get-caller-identity` works
- [ ] Stop/start instance from the CLI
- [ ] `labup` / `labdown` alias/script written
- [ ] Understand Elastic IP vs changing public IP; release unused EIPs

> 💸 Treat EC2 as metered, not free. Stop the instance when done. Watch for unattached/stopped-but-attached Elastic IPs (~$3.60/mo).

### Local virtualization
- [ ] Hypervisor installed (VirtualBox / Proxmox / VMware / UTM)
- [ ] Snapshot workflow practiced
- [ ] Fresh Ubuntu Server VM spun up from ISO in under 10 min
- [ ] Optional: mini-PC homelab box acquired

> Local stays because EC2 can't do: (1) the router capstone — no L2 broadcast domain, (2) promiscuous packet sniffing, (3) nested virtualization.

### Accounts
- [ ] GitHub account + SSH key
- [ ] AWS (or Azure) account, billing alarm at £5
- [ ] AWS Budgets configured
- [ ] Docker Hub account
- [ ] Datadog free trial (or OSS path: Grafana/Wazuh)

### Local tooling
- [ ] Terminal set up (WSL2+Terminal / iTerm2/Ghostty / native Linux)
- [ ] `git`, `curl`, `jq`, `ssh` installed
- [ ] Editor with terminal (VS Code or vim/neovim motions)
- [ ] Password manager + 2FA on GitHub and AWS root

### Notes repo
- [ ] Public `devops-journey` repo created
- [ ] Folders added: `01-linux/`, `02-bash/`, `03-networking/`, `04-git/`, `capstone/`, `cloud/`, `projects/`
- [ ] `README.md` committed with roadmap, boxes tracked publicly

**DoD:** Can create/snapshot/break/restore a VM without stress; public repo exists as future portfolio.

---

## STAGE 1 — Foundations

### 1A. Linux

**Task 1 — OverTheWire Bandit**
- [ ] Rules page read, SSH into Level 0
- [ ] `01-linux/bandit-notes.md` created
- [ ] Levels 0–5 cleared
- [ ] Levels 6–10 cleared
- [ ] Levels 11–15 cleared
- [ ] Levels 16–20 cleared
- [ ] Levels 21–26 cleared
- [ ] Levels 27–34 cleared
- [ ] One-line trick noted per level
- [ ] Summary written from memory (find, grep, xargs, nc, ssh -i, permissions, cron)
- [ ] Can explain the `2>/dev/null` redirect reasoning

**Task 2 — Own a box from the CLI only**
- [ ] Ubuntu Server launched via AWS CLI (not console)
- [ ] SSH in with key pair
- [ ] Noted what AWS handled for you (DHCP, DNS, route, firewall)
- [ ] Security group vs `ufw` understood
- [ ] Local VM install from ISO done too
- [ ] Static IP set at console; SSH in from host
- [ ] Compared local vs EC2 setup gaps
- [ ] Non-root user created, added to `sudo`
- [ ] `/etc/passwd`, `/etc/shadow`, `/etc/group` explained
- [ ] `chmod` octal + symbolic practiced
- [ ] `chown`, sticky bit / setuid / setgid explained
- [ ] SSH key pair generated (`ed25519`), copied with `ssh-copy-id`
- [ ] Key-based login confirmed before touching config
- [ ] `sshd_config` hardened (no passwords, no root login, custom port)
- [ ] sshd restarted, password auth verified rejected
- [ ] Second SSH session kept open during config edits
- [ ] `ufw` configured (default deny incoming, allow SSH)
- [ ] Enabled without lockout
- [ ] Underlying `iptables`/`nft` rules inspected
- [ ] nginx installed, custom page served
- [ ] `systemctl` status/start/stop/enable/restart fluent
- [ ] Custom systemd unit file written and enabled at boot
- [ ] Logs read via `journalctl`
- [ ] `apt` update/upgrade/search/show/remove practiced
- [ ] `unattended-upgrades` enabled
- [ ] Timezone + NTP sync confirmed
- [ ] Cron job and systemd timer created and compared

**Task 3 — Understand the machine**
- [ ] `top`/`htop` — load average explained
- [ ] `ps aux`/`ps -ef` — find & kill by PID
- [ ] `free -h` — free vs available vs buff/cache explained
- [ ] `df -h` vs `du -sh *` — disagreement case explained
- [ ] `iostat`, `vmstat`, `iotop` used
- [ ] `lsof -i` / `ss -tulpn` used
- [ ] `dmesg -T` read, OOM killer messages found
- [ ] `strace -p <pid>` used once
- [ ] CPU stress test diagnosed (`stress-ng`)
- [ ] Memory exhausted, OOM killer evidence found
- [ ] Disk filled (`fallocate`), service failure diagnosed
- [ ] Zombie/orphan process created and understood
- [ ] Logs filled, `logrotate` configured
- [ ] Port conflict diagnosed with `ss -tulpn`
- [ ] `/etc`, `/var`, `/usr`, `/opt`, `/proc`, `/tmp`, `/home` explained
- [ ] `/proc/<pid>/` explored
- [ ] Hard links vs symlinks tested

**DoD:** Can name the bottleneck (CPU/RAM/disk/IO/process/network) and prove it, with a personal `linux-triage.md`.

- [ ] **📣 Ship it** — LinkedIn post: broke Linux 6 ways on purpose

---

### 1B. Bash

**Prerequisite drills**
- [ ] Variables, quoting rules understood
- [ ] `if`/`case`/`for`/`while`/functions
- [ ] Exit codes, `$?`, `&&`, `||`
- [ ] Command substitution, positional args
- [ ] Redirection incl. here-docs
- [ ] `shellcheck` installed and used going forward
- [ ] `#!/usr/bin/env bash` + `set -euo pipefail` adopted as habit

**Task 1 — Backup script**
- [ ] Takes source + destination args
- [ ] Validates paths exist/are usable
- [ ] Timestamped tar.gz archive
- [ ] Rotation (keep last N)
- [ ] Logs each run with timestamps
- [ ] Distinct exit codes per failure
- [ ] Handles missing source / unwritable dest / disk full
- [ ] Lockfile prevents overlap
- [ ] `trap` cleans up temp files
- [ ] shellcheck-clean
- [ ] `--help` + argument parsing
- [ ] Scheduled via cron, then rewritten as systemd timer
- [ ] Verified ran unattended overnight
- [ ] Actually restored from a backup

**Task 2 — Log analysis tool**
- [ ] Top 10 IPs one-liner
- [ ] Top 10 paths one-liner
- [ ] Status code counts
- [ ] 4xx/5xx grouped per hour
- [ ] Total bytes transferred
- [ ] Requests from single IP in time order
- [ ] Unique user-agents sorted
- [ ] Scanner detection (`/admin`, `/wp-login.php`, `/.env`, `/.git/config`)
- [ ] Wrapped into `loganalyze.sh`
- [ ] Flags: `--top-ips`, `--status`, `--errors`, `--suspicious`, `--help`
- [ ] Handles gzipped logs
- [ ] Aligned column output
- [ ] shellcheck-clean
- [ ] Every awk field explained
- [ ] awk vs sed vs grep vs cut explained
- [ ] One pipeline rewritten with `sort -k`

**Task 3 — Idempotent hardening script**
- [ ] Functions per concern
- [ ] `main()` at bottom
- [ ] `set -euo pipefail`
- [ ] Refuses to run if not root
- [ ] `--dry-run` flag
- [ ] User only created if missing
- [ ] Config lines only appended if not already present
- [ ] Files backed up before editing
- [ ] Services only restarted if config changed
- [ ] Ran 3x on fresh VM — idempotency proven
- [ ] Creates admin user w/ SSH key
- [ ] Hardens sshd
- [ ] Configures ufw
- [ ] Installs & enables fail2ban sshd jail
- [ ] Enables unattended-upgrades
- [ ] Sets login banner
- [ ] Prints summary of changes

**DoD:** This script becomes the one you run on every new VM going forward.

- [ ] **📣 Ship it** — LinkedIn post: idempotent hardening script

---

### 1C. Networking

**Prerequisite theory**
- [ ] OSI vs TCP/IP models
- [ ] L2/L3/L4/L7 mapped
- [ ] TCP vs UDP, handshake, why DNS/video use UDP
- [ ] Public vs private IP ranges
- [ ] NAT explained
- [ ] DNS resolution order
- [ ] DNS record types (A, AAAA, CNAME, MX, TXT, NS, PTR)
- [ ] DHCP DORA sequence
- [ ] TLS handshake overview, cert chain

**Subnetting drills**
- [ ] /24, /16, /8 → dotted-decimal masks
- [ ] 192.168.1.0/26 breakdown
- [ ] 10.0.0.0/16 split into 4 subnets
- [ ] 172.16.34.200/20 network identified
- [ ] /31 and /32 explained
- [ ] 10/10 on random subnet quiz

**Task 1 — Wireshark: own traffic**
- [ ] Wireshark installed; capture vs display filters understood
- [ ] Plain HTTP capture: 3-way handshake identified
- [ ] GET request + raw headers found
- [ ] Follow TCP Stream used
- [ ] Connection teardown found
- [ ] TTL noted
- [ ] DNS capture: query/response matched by transaction ID
- [ ] Answer section, record type, TTL read
- [ ] NXDOMAIN capture
- [ ] TLS capture: ClientHello + SNI found
- [ ] ServerHello + cert found
- [ ] Application Data (opaque) point found
- [ ] Wrote down what an observer can/can't see over HTTPS
- [ ] Fluent in key filters (ip.addr, tcp.port, http.request.method, dns, syn scan, retransmission)

**Task 2 — Malicious pcap analysis**
- [ ] Infected host identified (IP/MAC/hostname)
- [ ] Timeline built
- [ ] Initial delivery found
- [ ] C2 server identified
- [ ] Beaconing characterized
- [ ] Suspicious HTTP objects exported
- [ ] User-Agent strings noted
- [ ] Exfiltration signs checked
- [ ] Incident report written (summary, timeline, IOCs, impact, recommendations)
- [ ] 3 different pcap exercises completed
- [ ] Answers checked against solutions

**Task 3 — Live tooling proof**
- [ ] `dig` sections read; `+trace` walked
- [ ] MX/TXT/NS/reverse lookups done
- [ ] Resolvers compared (8.8.8.8 vs 1.1.1.1 vs ISP)
- [ ] `/etc/resolv.conf` / systemd-resolved understood
- [ ] `ip a`, `ip r`, `ip neigh` reviewed
- [ ] `traceroute`/`mtr` run and explained
- [ ] Default gateway explained
- [ ] MTU/fragmentation discovered via ping sizes
- [ ] `ss -tulpn` reviewed
- [ ] `nc` chat + file transfer done
- [ ] `curl -v` full request/response read
- [ ] `openssl s_client` cert inspection
- [ ] `nmap -sn` host discovery (own lab only)
- [ ] `nmap -sV` service detection
- [ ] SYN vs full-connect scan explained
- [ ] Scan captured live in Wireshark
- [ ] "URL to Enter key" lifecycle explained in 2 min
- [ ] Status codes known (200/301/302/400/401/403/404/429/500/502/503/504)
- [ ] Reverse proxy vs forward proxy vs load balancer explained

**DoD:** Subnet math on paper, personal `networking-cheatsheet.md` written.

- [ ] **📣 Ship it** — LinkedIn post: nmap scan while capturing in Wireshark

---

### 1D. Git

**Task 1 — Beyond add/commit/push**
- [ ] Four areas explained (working dir, staging, local repo, remote)
- [ ] Commit model explained (snapshot + parent + hash)
- [ ] Branches as pointers, HEAD explained
- [ ] `git log --oneline --graph --all` read
- [ ] 5 messy commits squashed via `rebase -i`
- [ ] Commit reworded via rebase
- [ ] Commit dropped via rebase
- [ ] `git commit --amend` used
- [ ] Why never rewrite shared history — can explain
- [ ] Merge conflict created and resolved by hand
- [ ] Same conflict resolved via rebase
- [ ] Merge vs rebase tradeoff explained with a stance
- [ ] `reset --soft/--mixed/--hard` compared
- [ ] Hard-reset commit recovered via reflog
- [ ] Deleted branch restored from reflog
- [ ] `git stash` / `pop` / `list` used
- [ ] `git cherry-pick` used
- [ ] `git revert` used and compared to reset
- [ ] Bug planted 10 commits back, found via `git bisect run`
- [ ] `git blame` used
- [ ] `git log -S "string"` used

**Task 2 — Real collaboration flow**
- [ ] Repo created, `main` default
- [ ] Branch protection enabled (PR required, status check, no force-push)
- [ ] `PULL_REQUEST_TEMPLATE.md` added
- [ ] `CODEOWNERS` added
- [ ] `CONTRIBUTING.md` + real `README.md` added
- [ ] Feature branch naming convention used
- [ ] Conventional Commits used
- [ ] PR explaining *why* opened
- [ ] Review obtained
- [ ] Merge strategy compared and chosen with reason
- [ ] Release tagged with semver + notes
- [ ] GPG/SSH signing key generated
- [ ] `commit.gpgsign true` configured
- [ ] Public key added to GitHub
- [ ] Commits show Verified
- [ ] Supply-chain attack that signing prevents explained
- [ ] Trunk-based vs GitFlow vs GitHub Flow explained, one chosen and defended

**Task 3 — Secrets hygiene**
- [ ] Fake secret committed, 5 more commits on top
- [ ] Secret deleted in new commit, pushed
- [ ] Proven still recoverable via `git log -p | grep`
- [ ] `git-filter-repo` or BFG installed
- [ ] Secret removed from all history
- [ ] Rewritten history force-pushed
- [ ] Verified gone from history
- [ ] Noted: credential must still be rotated even after scrubbing
- [ ] `gitleaks` installed
- [ ] Pre-commit hook blocks secrets
- [ ] Tested — fake key rejected
- [ ] Comprehensive `.gitignore` written
- [ ] `.env.example` added
- [ ] GitHub secret scanning + push protection enabled

**DoD:** Can explain full incident-response sequence: rotate → scrub → investigate.

- [ ] **📣 Ship it** — LinkedIn post: leaked secret still recoverable after deletion

---

## 🏆 STAGE 1 CAPSTONE — Router + Sensor + SIEM + Automated Response

### Architecture
- [ ] Diagram drawn before building (WAN → router → LAN switch → hosts, sensor + SIEM marked)
- [ ] IP scheme planned
- [ ] Hardware decided (mini-PC or VM w/ 2 NICs)

### Part 1 — Router
- [ ] Two NICs present and named
- [ ] Static IP on LAN interface
- [ ] WAN gets address from home router
- [ ] Config persisted across reboot
- [ ] IP forwarding enabled and persisted
- [ ] IP forwarding kernel effect explained
- [ ] nftables masquerade rule written
- [ ] Default-deny INPUT, allow established/related
- [ ] FORWARD LAN→WAN allowed
- [ ] FORWARD WAN→LAN blocked except established
- [ ] Ruleset persisted across reboot
- [ ] INPUT/FORWARD/OUTPUT chains explained
- [ ] dnsmasq installed and configured on LAN interface
- [ ] DHCP range/lease/gateway/DNS set
- [ ] Static lease for one host by MAC
- [ ] Local DNS entry added
- [ ] DORA sequence observed in dnsmasq log
- [ ] Second device gets IP from your router
- [ ] Internet reachable through your NAT
- [ ] Traceroute shows your router as hop 1
- [ ] Firewall rule blocking a site tested then removed
- [ ] Reboot survives, everything comes back
- [ ] Level-up: VLANs added (trusted/untrusted)
- [ ] Level-up: rebuilt with VyOS/OPNsense + comparison writeup

### Part 2 — Sensor
- [ ] Manual `tcpdump` capture done and opened in Wireshark
- [ ] tcpdump filters practiced (host/port/protocol)
- [ ] Promiscuous mode understood
- [ ] Suricata and/or Zeek installed, pointed at LAN interface
- [ ] Emerging Threats ruleset pulled and updated
- [ ] `eve.json` / Zeek logs confirmed writing
- [ ] False positives tuned out
- [ ] EICAR test file triggers alert
- [ ] nmap from one host triggers port-scan alert
- [ ] Bad-domain DNS request logged
- [ ] Custom Suricata rule written and fired

**DoD:** Sensor produces real timestamped events, triggerable on demand.

### Part 3 — nginx → SIEM → analytics → automated response
- [ ] App deployed on lab host
- [ ] nginx reverse proxy in front
- [ ] TLS enabled (self-signed or Let's Encrypt)
- [ ] JSON access/error logging enabled
- [ ] Rate limiting + security headers added
- [ ] SIEM chosen (Datadog / Wazuh / Grafana+Loki+Promtail / ELK)
- [ ] Agent/shipper installed on router + nginx host
- [ ] nginx logs shipped
- [ ] Suricata/Zeek logs shipped
- [ ] System logs shipped (auth.log)
- [ ] Logs confirmed parsed into fields
- [ ] Dashboard: requests/sec over time
- [ ] Dashboard: status code breakdown
- [ ] Dashboard: top source IPs
- [ ] Dashboard: top requested paths
- [ ] Dashboard: geo map of source IPs
- [ ] Dashboard: IDS alert volume by severity
- [ ] Dashboard: failed SSH logins over time
- [ ] Dashboard: p95/p99 latency
- [ ] Detection: scanner/credential stuffing (>50 4xx/min)
- [ ] Detection: brute force (>10 failed SSH/5min)
- [ ] Detection: 5xx rate threshold
- [ ] Detection: high-severity IDS alert
- [ ] Detection: sensitive path probing
- [ ] Alerts routed to Slack/Discord
- [ ] Responder script blocks an IP
- [ ] Webhook listener exposed safely, authenticated
- [ ] SIEM monitor → webhook → responder wired
- [ ] Allowlist protects your own management IP
- [ ] Blocks auto-expire after N minutes
- [ ] Every automated action logged with reason + alert ID
- [ ] Slack/Discord notification on block
- [ ] Attack script written (path enum + SSH brute force)
- [ ] Full loop run and confirmed: traffic → log → SIEM → alert → webhook → block → notify
- [ ] Attacking host confirmed blocked
- [ ] Loop screenshotted/recorded
- [ ] Time-to-block measured

### Write-up
- [ ] Architecture diagram
- [ ] Detection→response sequence diagram
- [ ] Config committed (secrets excluded)
- [ ] README a stranger could follow
- [ ] Blog post written

**CAPSTONE DoD:** Scripted attack against your own infra gets auto-blocked with a chat alert, demoable live.

- [ ] **📣 Ship it** — the big LinkedIn post (demo GIF, hook, diagram, repo link, cross-post to r/homelab + r/devops)

### Part 4 (bonus) — Cloud version of the router
- [ ] VPC with public + private subnet in Terraform
- [ ] NAT instance (not NAT Gateway) launched in public subnet
- [ ] Source/dest check disabled on ENI, explained
- [ ] IP forwarding + masquerade rules replicated
- [ ] Private subnet route table points at NAT instance ENI
- [ ] Private host reaches internet through your NAT instance
- [ ] VPC Flow Logs enabled, shipped to SIEM
- [ ] Comparison written: hand-rolled vs VPC routing vs NAT Gateway
- [ ] Destroyed when done
- [ ] **📣 Post on LinkedIn** — router built twice comparison

---

## STAGE 2 — Cloud & Scale

> 💸 Billing alarm at £5. Free-tier types only. `terraform destroy` every session. Never leave NAT Gateway/ALB/EKS running overnight.

### 2A. Containers & Docker

**Prerequisite concepts**
- [ ] Container = namespaces + cgroups + layered FS — explained
- [ ] Container vs VM explained
- [ ] Images vs containers vs registries explained
- [ ] Layers + build cache explained

**Basics**
- [ ] `run/ps/logs/exec -it/stop/rm/images/rmi` fluent
- [ ] Ran interactively, poked around inside
- [ ] Port mapping order understood
- [ ] Volumes vs bind mounts understood
- [ ] `docker inspect` / `docker stats` used

**Build a real image**
- [ ] Dockerfile written for an app
- [ ] FROM/RUN/COPY/WORKDIR/ENV/EXPOSE/CMD vs ENTRYPOINT understood
- [ ] `.dockerignore` added
- [ ] Layers ordered for cache efficiency
- [ ] Built, tagged properly, pushed to registry

**Compose**
- [ ] `docker-compose.yml` with frontend+backend+db
- [ ] Named volumes persist DB data
- [ ] Custom network for name resolution
- [ ] `depends_on` + healthchecks correct
- [ ] `.env` file used (gitignored)
- [ ] `up -d`, `logs -f`, `down -v` fluent

**Hardening**
- [ ] Multi-stage build
- [ ] Minimal base (alpine/slim/distroless)
- [ ] Base image pinned by digest
- [ ] Non-root user created and verified
- [ ] `HEALTHCHECK` added
- [ ] `--read-only` + `--cap-drop=ALL` where possible
- [ ] No secrets baked in — verified via `docker history`
- [ ] Trivy scan clean or consciously accepted
- [ ] hadolint findings fixed
- [ ] Image size before/after recorded

**DoD:** Non-root, Trivy-clean, minimal image size, namespaces/cgroups explainable.

- [ ] **📣 Ship it** — LinkedIn post: image size before/after

---

### 2B. Cloud (AWS or Azure)

**Prerequisite concepts**
- [ ] Shared responsibility model explained
- [ ] Regions vs AZs, multi-AZ importance
- [ ] IAM mental model (users/groups/roles/policies), roles > keys
- [ ] Least privilege explained

**Account hygiene**
- [ ] MFA on root, root never used again
- [ ] Admin IAM user created w/ MFA
- [ ] Billing alarm at £5
- [ ] CloudTrail enabled
- [ ] GuardDuty enabled
- [ ] AWS Config / Security Hub baseline checked

**Networking**
- [ ] VPC with sensible CIDR
- [ ] 2 public subnets across AZs
- [ ] 2 private subnets across AZs
- [ ] IGW + public route table
- [ ] NAT Gateway + private route table (destroy after)
- [ ] Security groups scoped to minimum needed
- [ ] SGs vs NACLs explained

**Compute + LB**
- [ ] EC2 in private subnet
- [ ] Dockerized app deployed (or ECS Fargate)
- [ ] ALB in public subnets
- [ ] Target group + passing health checks
- [ ] HTTPS listener w/ ACM cert, HTTP→HTTPS redirect
- [ ] App reachable over HTTPS; instance not directly internet-reachable

**Identity & secrets**
- [ ] IAM role for EC2/ECS task, least privilege
- [ ] Attached via instance profile, no access keys on box
- [ ] DB credentials in Secrets Manager/Parameter Store
- [ ] App fetches secrets at runtime via role
- [ ] Repo grepped for credentials — zero hits

**Storage & data**
- [ ] S3 bucket: block public access, versioning, encryption
- [ ] Bucket policy scoped to your role
- [ ] RDS in private subnet, encrypted, not public
- [ ] Automated backups enabled

**Observability**
- [ ] Logs shipping to CloudWatch
- [ ] CloudWatch alarm on CPU/error rate
- [ ] Own activity found in CloudTrail
- [ ] GuardDuty sample finding triggered and read

**Clean up**
- [ ] NAT Gateway, ALB, RDS, EC2 deleted
- [ ] Bill confirmed back to ~£0

**DoD:** HTTPS-reachable app, private compute/DB, zero hardcoded credentials.

- [ ] **📣 Ship it** — LinkedIn post: VPC diagram + no-credentials-in-code note

---

### 2C. Terraform

**Prerequisite concepts**
- [ ] Declarative vs imperative explained
- [ ] State file purpose, why it's never in git
- [ ] plan → apply → destroy cycle
- [ ] Providers/resources/data sources/variables/outputs/locals

**Fundamentals**
- [ ] Terraform installed; init/plan/apply/destroy on trivial resource
- [ ] Plan output read carefully (+/-/~/-/+)
- [ ] `fmt` + `validate` in workflow
- [ ] Typed variables w/ descriptions and defaults
- [ ] Outputs surface ALB DNS name
- [ ] Data source used (e.g. latest AMI lookup)

**Remote state**
- [ ] S3 bucket for state, versioned + encrypted
- [ ] DynamoDB table for locking
- [ ] Backend configured, local state migrated
- [ ] `*.tfstate*` and `.terraform/` gitignored

**Professional structure**
- [ ] Split into main/variables/outputs/providers/versions.tf
- [ ] Provider + Terraform version pinned
- [ ] Own module written (e.g. modules/vpc)
- [ ] Module reused twice with different vars
- [ ] Environments separated (envs/dev, envs/prod)
- [ ] Resources tagged consistently via default_tags

**Recreate 2B in code**
- [ ] VPC, subnets, IGW, NAT, route tables
- [ ] Security groups
- [ ] ALB, target group, listeners, ACM cert
- [ ] EC2/ECS w/ IAM role
- [ ] S3 and RDS
- [ ] Secrets Manager entries (values injected, not committed)

**IaC scanning**
- [ ] tfsec (or Trivy config) run, findings fixed
- [ ] Checkov run, findings fixed or documented
- [ ] terraform-docs added
- [ ] pre-commit config running fmt/validate/tfsec

**The proof**
- [ ] `terraform destroy` — everything gone
- [ ] `terraform apply` — everything back from nothing
- [ ] Done twice
- [ ] Manual console drift detected via `plan`

**DoD:** Entire environment rebuilds from zero with one command.

- [ ] **📣 Ship it** — LinkedIn post: destroy → rebuild GIF

---

### 2D. Kubernetes

**Prerequisite concepts**
- [ ] Control plane: API server, etcd, scheduler, controller manager
- [ ] Node components: kubelet, kube-proxy, container runtime
- [ ] Reconciliation loop explained
- [ ] Pods vs ReplicaSets vs Deployments vs Services
- [ ] Why bare Pods are avoided

**Local cluster**
- [ ] kind/minikube cluster running
- [ ] `get/describe/logs/exec/apply/delete` fluent
- [ ] `kubectl explain` used
- [ ] Bare Pod deployed, deleted, nothing self-heals
- [ ] Deployment self-heals a deleted Pod

**Core workloads**
- [ ] Deployment w/ 3 replicas
- [ ] ClusterIP Service + internal DNS
- [ ] NodePort/LoadBalancer difference understood
- [ ] Ingress + controller w/ host-based routing
- [ ] ConfigMap (env vars + file mount)
- [ ] Secret used, base64-is-not-encryption understood
- [ ] Namespaces separating environments

**Production-readiness**
- [ ] Resource requests/limits on every container
- [ ] Liveness vs readiness probes
- [ ] Rolling update strategy (maxSurge/maxUnavailable)
- [ ] Zero-downtime rolling update proven under load
- [ ] `kubectl rollout undo` used
- [ ] HorizontalPodAutoscaler scaling out/in
- [ ] PersistentVolumeClaim for stateful data
- [ ] StatefulSet for DB, reasoning explained

**Security hardening**
- [ ] securityContext hardened (non-root, read-only FS, no priv escalation, drop caps)
- [ ] NetworkPolicies default-deny + explicit allow, proven
- [ ] RBAC: ServiceAccount + Role + RoleBinding scoped
- [ ] Pod Security Standards (restricted) enforced
- [ ] Falco installed
- [ ] Falco alert triggered on purpose
- [ ] Manifests scanned w/ kubesec/Checkov

**Packaging & managed cluster**
- [ ] Helm chart w/ templated values
- [ ] Install/upgrade/rollback via Helm
- [ ] Deployed to EKS/AKS via Terraform
- [ ] Managed cluster destroyed when idle

**DoD:** Self-heals, scales, zero-downtime updates, NetworkPolicies proven, Falco catches misbehavior.

- [ ] **📣 Ship it** — LinkedIn post: pod-kill loop + zero failed requests, then NetworkPolicy post

---

### 2E. CI/CD & System Design

**CI/CD pipeline**
- [ ] CI vs CD (delivery) vs CD (deployment) explained
- [ ] GitHub Actions workflow on PR trigger
- [ ] Steps: checkout→lint→test→build→scan→push
- [ ] Dependency caching
- [ ] Matrix build across versions
- [ ] OIDC federation to AWS (no long-lived keys)
- [ ] PR check made required
- [ ] Manual approval gate before prod
- [ ] Blue/green vs canary vs rolling explained

**System design**
- [ ] Design doc: LB, horizontal scaling, statelessness, caching, DB replicas, queues
- [ ] Failure modes covered (AZ death, DB failover, dependency timeout, circuit breakers)
- [ ] SLIs/SLOs/error budget defined
- [ ] Threat model added (trust boundaries, entry points, blast radius)
- [ ] Architecture diagram drawn
- [ ] Presented out loud in 10 minutes

**DoD:** Doc + diagram defensible in a senior interview.

- [ ] **📣 Ship it** — LinkedIn post: architecture diagram + threat model

---

## 🚀 STAGE 2 FLAGSHIP — The Self-Defending GitOps Platform

### Phase 1 — Foundation
- [ ] App picked (real auth + DB + API, not a to-do list)
- [ ] Repo structure: app/, infra/, k8s/, .github/workflows/, docs/
- [ ] README with architecture diagram at top
- [ ] Terraform provisions VPC+EKS+ECR+RDS+IAM, remote state, modular
- [ ] `terraform apply` from zero works

### Phase 2 — Secure pipeline (every PR passes, in order)
- [ ] gitleaks
- [ ] Semgrep (SAST)
- [ ] Unit + integration tests w/ coverage gate
- [ ] Dependency scan (npm audit/pip-audit/Dependabot/Renovate)
- [ ] tfsec + Checkov
- [ ] hadolint
- [ ] Multi-stage, non-root, distroless image build
- [ ] Trivy scan, fail build on HIGH/CRITICAL
- [ ] SBOM generated (Syft), attached as artifact
- [ ] Image signed w/ cosign/sigstore (keyless OIDC)
- [ ] Pushed to ECR w/ immutable git-SHA tag
- [ ] All AWS auth via OIDC, zero long-lived creds

### Phase 3 — GitOps deployment
- [ ] Argo CD (or Flux) installed
- [ ] Config repo/dir as single source of truth
- [ ] Pipeline updates image tag, Argo syncs
- [ ] Drift + self-heal proven (manual delete → comes back)
- [ ] Kyverno/OPA Gatekeeper admission policies (unsigned images, privileged pods, root, no limits — all rejected)
- [ ] Unsigned image deploy rejected + screenshotted

### Phase 4 — Observability & runtime security
- [ ] Prometheus + Grafana (or Datadog) — RED metrics
- [ ] Logs to Loki or Stage 1 SIEM
- [ ] OpenTelemetry tracing
- [ ] Dashboards: health, deploy markers, errors, latency percentiles
- [ ] Falco alerts routed to Slack
- [ ] Alerting thresholds tuned (no fatigue)
- [ ] K8s audit logs to SIEM

### Phase 5 — Attack it
- [ ] Build-time proof: vulnerable dep + hardcoded secret PR blocked, screenshotted
- [ ] Admission proof: unsigned image + root pod rejected, screenshotted
- [ ] Runtime proof: scripted attack (path enum, brute force, unexpected shell)
- [ ] Falco + SIEM detect it, Slack alerts fire
- [ ] Automated response: IP blocked and/or compromised pod killed+replaced
- [ ] Optional: auto-rollback on error-rate spike
- [ ] Whole thing recorded as demo GIF

### Phase 6 — Ship the story
- [ ] Architecture, pipeline, and detection→response diagrams
- [ ] README opens with what/why, then how to run
- [ ] `docs/SECURITY.md` listing every control + threat addressed
- [ ] Blog post on the build
- [ ] Demo video/GIF in README
- [ ] **📣 THE LinkedIn post** — demo GIF, diagram, hook, every tool named, honest "what broke," repo link, cross-post to r/devops + r/kubernetes, follow-up comment a week later

**FLAGSHIP DoD:** A stranger reads the README, understands the system in 2 minutes, sees proof both layers work.

---

## STAGE 3 — Real-World Cybersec Projects (pick 2–3)

### Project A — Supply-chain security lab
- [ ] Cosign keyless signing in pipeline
- [ ] Kyverno rejects unsigned images
- [ ] SBOM (Syft) + scan (Grype) every build
- [ ] SLSA provenance attestation attached
- [ ] Demo: unsigned image rejected at admission
- [ ] Writeup: SolarWinds/xz-utils style attack defended against
- [ ] **📣 Post on LinkedIn**

### Project B — Secrets management with Vault
- [ ] Vault deployed (dev mode → real config w/ unsealing)
- [ ] Database secrets engine — dynamic short-lived creds
- [ ] Automatic rotation configured
- [ ] Kubernetes auth method (ServiceAccount-based)
- [ ] Vault Agent Injector delivers secrets to pods
- [ ] Flagship app refactored — zero static secrets
- [ ] Audit log on every secret access
- [ ] Demo: credential expiring + reissued automatically
- [ ] **📣 Post on LinkedIn**

### Project C — Full home SOC
- [ ] Stage 1 capstone extended across Linux + Windows endpoints
- [ ] Wazuh agents everywhere (FIM, rootkit checks, vuln detection)
- [ ] Suricata IDS on router
- [ ] Detections mapped to MITRE ATT&CK
- [ ] Full intrusion simulated (access→persistence→privesc→exfil)
- [ ] Each stage detected + documented
- [ ] IR playbooks written (detect→triage→contain→eradicate→recover→lessons)
- [ ] Purple-team exercise run (attack, detect, improve, repeat)
- [ ] **📣 Post on LinkedIn**

### Project D — Cloud incident-response automation
- [ ] GuardDuty enabled, sample findings generated
- [ ] EventBridge rule on high-severity findings
- [ ] Lambda: quarantine SG, snapshot EBS, tag compromised, notify
- [ ] All in Terraform
- [ ] Tested with real sample finding, containment screenshotted
- [ ] Human runbook documented
- [ ] **📣 Post on LinkedIn**

### Project E — Policy as code / compliance
- [ ] OPA/Conftest validating Terraform plans in CI
- [ ] Kyverno enforcing at K8s admission
- [ ] Policies: no public S3, encryption required, mandatory tags, no privileged pods, no `latest`, resource limits
- [ ] Same policies enforced in both CI and admission
- [ ] Compliance report mapped to CIS Benchmark
- [ ] Demo: bad config rejected at both layers
- [ ] **📣 Post on LinkedIn**

### Project F — Offensive practice
- [ ] TryHackMe DevSecOps + SOC Level 1 paths
- [ ] HackTheBox cloud/container challenges
- [ ] flAWS.cloud + flaws2.cloud
- [ ] Kubernetes Goat — exploited, then every finding fixed
- [ ] OWASP Juice Shop — Top 10 worked hands-on
- [ ] Writeup per challenge: exploit + prevention
- [ ] **📣 Post on LinkedIn**

---

## The Meta-Game — Standing Out

### Portfolio
- [ ] Every project README has diagram + demo GIF + clear "why"
- [ ] Pinned repos ordered by impressiveness
- [ ] Green contribution graph reflects real work
- [ ] One blog post per capstone

### Certifications
- [ ] AWS Solutions Architect Associate (after 2B/2C)
- [ ] CKA (after 2D)
- [ ] CompTIA Security+ (after Stage 3)
- [ ] Optional: CKS, AWS Security Specialty, Terraform Associate

### Community & visibility
- [ ] OSS contribution (even a docs PR)
- [ ] Active in CNCF / r/devops / K8s Slack
- [ ] Flagship posted to LinkedIn + r/devops
- [ ] One meetup or conference attended

### Interview readiness
- [ ] Can explain every project in depth, incl. what went wrong
- [ ] Can whiteboard a scalable architecture live
- [ ] Can do "URL to Enter key" in 2 minutes
- [ ] Can debug a broken deployment out loud
- [ ] Has stories: production incident, tough tradeoff, being wrong
- [ ] Can always answer "why," including the option rejected

---

## Final Checkpoint — Is He a Hero Yet?

- [ ] Can rebuild any environment from code, one command
- [ ] Can diagnose a broken system by reasoning, not googling verbatim
- [ ] Has automated something from an hour down to zero
- [ ] Has caught and auto-responded to an attack in his own logs
- [ ] Has a public portfolio demonstrating all of the above
- [ ] Can teach any section of this roadmap to someone else

> **North star:** Traffic → observe → detect → respond → automate. First in a homelab, then in the cloud, with security in the pipeline instead of bolted on after.

---

## 📣 Appendix — LinkedIn Post Template

**Structure:**
1. **Hook** (line 1 — lead with result/surprise, never "excited to share")
2. **What was built** — 2–3 plain-English sentences
3. **What broke** — the most valuable paragraph, makes it read as real
4. **What was learned** — the transferable insight
5. **Tools used** — named explicitly
6. **Link** — repo/blog

**Rules:**
- [ ] Always include a visual (diagram, GIF, screenshot)
- [ ] Link in first comment if reach matters
- [ ] 3–5 hashtags, not fifteen
- [ ] Short paragraphs, whitespace
- [ ] No emoji spam / no "thrilled to announce"
- [ ] Reply to every comment
- [ ] Post consistently, ~one per project

**Skeleton:**
```
[Surprising result or concrete number, one sentence.]

I built [what it is] using [3–4 headline tools].

The part that took longest: [what broke, why].
[What it turned out to be. What he'd do differently.]

What I actually took away from it: [the transferable insight].

Full writeup and code: [link in comments]

#DevOps #[Tool] #CloudSecurity
```

**Also worth doing:**
- [ ] Update LinkedIn headline as he progresses
- [ ] Add each project to the Projects section of the profile
- [ ] Pin the flagship post
- [ ] Turn best 2–3 posts into full blog articles later
