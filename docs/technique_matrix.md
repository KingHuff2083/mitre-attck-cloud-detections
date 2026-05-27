# Technique Reference Matrix

Detailed reference for all 15 techniques, log sources, and tuning guidance.

---

## Detection Engineering Notes

### Tuning philosophy
Each detection is written with a **low false-positive bias** by including:
- Thresholds (count-based, volume-based) rather than single-event triggers
- Exclusion filters for known-good identities (CI/CD roles, service accounts)
- Time-window aggregation to distinguish anomalous from routine behavior

### False positive management
Before deploying any rule to production:
1. Run in **detect-only / alert** mode for 7 days
2. Triage every alert and document as TP, FP, or Benign-True-Positive (BTP)
3. Add exclusion filters for recurring FP patterns
4. Set alert threshold based on observed baseline

---

## Log Source Requirements

| Log Source | What it Covers | AWS | Azure | GCP |
|---|---|---|---|---|
| CloudTrail | All AWS API calls | ✅ | — | — |
| AuditLogs | Azure AAD operations | — | ✅ | — |
| StorageBlobLogs | Blob/S3 access patterns | ✅ | ✅ | — |
| AzureActivity | Azure resource operations | — | ✅ | — |
| VPC Flow Logs | Network traffic metadata | ✅ | ✅ (NSG) | ✅ |
| KubePodInventory | AKS/EKS workload state | ✅ | ✅ | ✅ |
| ContainerRegistryLogs | Image push/pull events | ✅ | ✅ | ✅ |

---

## Severity Rationale

| Severity | Criteria | Count |
|---|---|---|
| Critical | Near-certain attacker behavior; immediate action required | 1 |
| High | Strong signal; likely adversary activity; investigate promptly | 6 |
| Medium | Moderate signal; could be legitimate; review in triage queue | 8 |
| Low | Weak signal; useful for correlation; review daily | 0 |

---

## NIST 800-53 Cross-Reference

| Control | Techniques Addressed |
|---|---|
| AC-2 (Account Management) | T1078.004, T1136.003, T1098.001 |
| AC-6 (Least Privilege) | T1078.004, T1098.001, T1552.005 |
| AU-9 (Protection of Audit Info) | T1562.008 |
| AU-12 (Audit Record Generation) | All techniques |
| CM-7 (Least Functionality) | T1525, T1610, T1611 |
| CM-8 (System Component Inventory) | T1580, T1535 |
| SC-7 (Boundary Protection) | T1048 |
| SC-28 (Protection at Rest) | T1530, T1619 |
| SI-4 (System Monitoring) | All techniques |

---

*Author: Odis Huff III | github.com/KingHuff2083*
