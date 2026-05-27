# 🎯 MITRE ATT&CK Cloud Detection Coverage Map

![ATT&CK Version](https://img.shields.io/badge/ATT%26CK-v14-red?style=flat-square&logo=mitre)
![Techniques](https://img.shields.io/badge/Techniques-15-blue?style=flat-square)
![Sigma Rules](https://img.shields.io/badge/Sigma%20Rules-15-purple?style=flat-square)
![KQL Queries](https://img.shields.io/badge/KQL%20Queries-15-cyan?style=flat-square)
![Coverage](https://img.shields.io/badge/Coverage-100%25-brightgreen?style=flat-square)
![CI](https://img.shields.io/github/actions/workflow/status/KingHuff2083/mitre-attck-cloud-detections/validate.yml?label=sigma%20validation&style=flat-square)

Detection engineering portfolio mapping **15 cloud-relevant MITRE ATT&CK techniques** to production-grade Sigma rules and Azure Sentinel KQL queries across AWS, Azure, and GCP environments.

> 🔗 **Part of my detection & response portfolio** — [cloud-security-portfolio](https://github.com/KingHuff2083/cloud-security-portfolio)

---

## 📺 Live Dashboard

Open [`dashboard.html`](./dashboard.html) in your browser for an interactive coverage map with:
- Tactic heatmap showing detection distribution
- Sortable / filterable technique table
- Expandable rows with false positive guidance, NIST controls, and rule links

**To host on GitHub Pages:**
1. Repo Settings → Pages → Source: `main` branch, `/ (root)`
2. Visit `https://KingHuff2083.github.io/mitre-attck-cloud-detections/dashboard.html`

---

## 📁 Repository Structure

```
mitre-attck-cloud-detections/
├── dashboard.html                        # Interactive HTML coverage dashboard
├── README.md                             # You are here
├── sigma/                                # Sigma detection rules (platform-agnostic)
│   ├── T1078_valid_accounts_cloud.yml    # Valid Accounts: Cloud Accounts
│   ├── T1136_create_account_cloud.yml    # Create Account: Cloud Account
│   ├── T1530_T1537_T1562_cloud.yml       # Data Exfil + Disable Logging
│   ├── T1098_T1580_T1619_cloud.yml       # Account Manipulation + Discovery
│   ├── T1552_T1525_T1610_T1611_cloud.yml # Credentials + Container techniques
│   └── T1535_T1048_cloud.yml             # Unused Regions + Alt Protocol Exfil
├── kql/
│   └── all_detections.kql               # Azure Sentinel KQL — all 15 techniques
├── docs/
│   └── technique_matrix.md              # Tuning guide, log sources, NIST mapping
└── .github/
    └── workflows/
        └── validate.yml                 # CI: Sigma rule validation on every push
```

---

## 🎯 Techniques Covered

| ID | Name | Tactic | Severity | Log Source |
|---|---|---|---|---|
| T1078.004 | Valid Accounts: Cloud Accounts | Initial Access | Medium | CloudTrail, SigninLogs |
| T1136.003 | Create Account: Cloud Account | Persistence | Medium | CloudTrail, AuditLogs |
| T1530 | Data from Cloud Storage | Collection | High | CloudTrail, StorageBlobLogs |
| T1537 | Transfer Data to Cloud Account | Exfiltration | High | CloudTrail, AzureActivity |
| **T1562.008** | **Impair Defenses: Disable Cloud Logs** | **Defense Evasion** | **Critical** | **CloudTrail, AzureActivity** |
| T1098.001 | Account Manipulation: Additional Credentials | Persistence | High | CloudTrail, AuditLogs |
| T1580 | Cloud Infrastructure Discovery | Discovery | Medium | CloudTrail, AzureActivity |
| T1619 | Cloud Storage Object Discovery | Discovery | Medium | CloudTrail, StorageBlobLogs |
| T1552.005 | Unsecured Credentials: Cloud Metadata | Credential Access | Medium | CloudTrail, AzureActivity |
| T1525 | Implant Container Image | Persistence | Medium | CloudTrail, ContainerRegistry |
| T1610 | Deploy Container | Execution | Medium | CloudTrail, AzureActivity |
| T1611 | Escape to Host | Privilege Escalation | High | KubePodInventory |
| T1535 | Unused Cloud Regions | Defense Evasion | High | CloudTrail, AzureActivity |
| T1048 | Exfiltration Over Alternative Protocol | Exfiltration | Medium | VPC Flow Logs |
| T1190 | Exploit Public-Facing Application | Initial Access | High | WAF Logs, CloudTrail |

---

## 🔧 Using the Sigma Rules

Sigma rules are platform-agnostic and convert to any SIEM using [sigma-cli](https://github.com/SigmaHQ/sigma-cli):

```bash
# Install sigma-cli
pip install sigma-cli

# Convert a rule to Splunk SPL
sigma convert -t splunk sigma/T1562_T1530_T1537_cloud.yml

# Convert to Azure Sentinel KQL
sigma convert -t sentinel sigma/T1562_T1530_T1537_cloud.yml

# Convert to Elastic (EQL)
sigma convert -t elasticsearch sigma/T1562_T1530_T1537_cloud.yml

# Validate all rules
sigma check sigma/
```

---

## 🔍 Deploying the KQL Rules (Azure Sentinel)

1. Open **Microsoft Sentinel → Analytics → + Create → Scheduled query rule**
2. Copy the relevant query from `kql/all_detections.kql`
3. Set query frequency and lookback period per the comment header in each rule
4. Map entities (UserPrincipalName, IPAddress) for investigation graph
5. Set incident creation and alert grouping

---

## 📊 Tactic Coverage

```
Initial Access      ████████ 2 techniques  (T1078.004, T1190)
Persistence         ████████████ 3          (T1136.003, T1098.001, T1525)
Defense Evasion     ████████ 2              (T1562.008, T1535)
Discovery           ████████ 2              (T1580, T1619)
Exfiltration        ████████ 2              (T1537, T1048)
Collection          ████ 1                  (T1530)
Credential Access   ████ 1                  (T1552.005)
Execution           ████ 1                  (T1610)
Privilege Escalation████ 1                  (T1611)
```

---

## 🏛️ Framework Alignment

| Framework | Controls / References |
|---|---|
| MITRE ATT&CK for Cloud | v14 — IaaS, SaaS, Azure AD |
| NIST 800-53 Rev 5 | AC-2, AC-6, AC-7, AU-9, AU-12, CM-7, CM-8, IA-5, RA-5, SC-7, SC-28, SI-2, SI-3, SI-4, SI-10 |
| DoD 8140 DCWF | Cyber Defense Analyst (511), Incident Responder (531) |
| CIS Cloud Benchmarks | AWS CIS 1.4, Azure CIS 1.5, GCP CIS 1.3 |

---

## 🚀 Push to GitHub

```bash
# Clone or init
git init
git add .
git commit -m "feat: MITRE ATT&CK cloud detection coverage — 15 techniques, Sigma + KQL"

# Create repo at github.com/KingHuff2083/mitre-attck-cloud-detections first, then:
git remote add origin https://github.com/KingHuff2083/mitre-attck-cloud-detections.git
git branch -M main
git push -u origin main
```

**Recommended GitHub repo settings:**
- Description: `15 cloud ATT&CK detection rules — Sigma + KQL — AWS · Azure · GCP`
- Topics: `mitre-attack`, `sigma`, `kql`, `detection-engineering`, `cloud-security`, `aws`, `azure`, `sentinel`, `threat-detection`
- Enable GitHub Pages → source: `main` branch → serves `dashboard.html` as live URL

---

## ✍️ Author

**Odis Huff III** — Cloud Security Engineer | Oakland, CA

[![GitHub](https://img.shields.io/badge/GitHub-KingHuff2083-black?style=flat-square&logo=github)](https://github.com/KingHuff2083)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-odishuffiii-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/odishuffiii)
[![Portfolio](https://img.shields.io/badge/Portfolio-cloud--security--portfolio-orange?style=flat-square)](https://github.com/KingHuff2083/cloud-security-portfolio)

---

*MITRE ATT&CK® is a registered trademark of The MITRE Corporation.*
