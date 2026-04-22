# Vendor Security Review — Decision Tool

**Author:** Becky Gaylord, GRC Manager
**Live tool:** https://beckyg-flex.github.io/vrm-decision-tree/
**Notion page:** https://www.notion.so/34a4b351646a8182be61e0101a7fc902
**Source file:** `index.html` (self-contained, no dependencies)

---

## What this is

An interactive decision tool for Compliance (Danyale Jensen) to determine whether and how Security should be involved in a vendor review.

Built April 2026 as the primary deliverable of SEC-2510 (Build Security Intelligence Layer for Vendor Risk Management Program), under epic SEC-2750 (Operationalize Security's Role in Vendor Risk Management).

**Context:** Flex's existing VDD process does not have a defined Security entry point. Danyale tags Becky informally for T1/critical vendors but doesn't have a documented standard for when or how. This tool codifies the trigger system based on an analysis of all 43 completed VDDs in the library.

---

## How to use it

Open the live URL. Answer three questions about the VDD in progress:

1. **Record quality:** Does the VDD have data quality issues (broken formula, N/A due diligence, stale SOC 2)?
2. **Security signals:** Does the vendor have T1 integration, AI/ML use, breach history, litigation, or dual regulatory exposure?
3. **High-exposure combinations:** Does the vendor have any of the higher-risk signal combinations that require a written narrative?

The tool outputs one of four results:

| Result | Meaning |
|--------|---------|
| Fix First (Level 0) | VDD has data quality issues; correct before Security can review |
| No Action | No security signals; close the VDD |
| Level 1 | Tag @Becky; Security sends 3-5 targeted questions (5-day turnaround) |
| Level 2 | Tag @Becky + Victoria (Legal); Security writes full risk narrative (10-day turnaround) |

---

## Trigger logic

### Level 0 (data quality flags — fire on all VDDs)

- Due Diligence = N/A or blank
- Residual Risk Rating = FALSE or blank (formula broken)
- SOC 2 older than 18 months, or VDD notes "old SOC 2"

### Level 1 (any of these = tag Security)

- PII = Yes AND (BSA/AML = Yes OR Fraud = Yes)
- Security Tier = T1 (direct API or database integration)
- Vendor uses AI, ML, or automated decision-making
- SOC 2 has management responses or exceptions
- Breach or active litigation mentioned in VDD rationale
- Pilot/Trial vendor with T1 tier already assigned

### Level 2 (any of these = tag Security + Legal)

- PII = Yes AND BCP = Yes AND Security Tier = T1
- BSA/AML = Yes AND Fraud = Yes
- Outside US = Yes AND customer PII in scope
- AI/ML vendor AND customer PII in scope
- Direct access to Flex production systems
- Active litigation involving data security

---

## Regulatory basis

The trigger system maps directly to compliance requirements:

- **NYDFS Part 500 §500.11:** Written TPSP security policy, minimum contract security clauses (72-hour breach notification, MFA, right to audit, data destruction, annual compliance rep, subprocessor notification). None of these are currently verified in any VDD.
- **SOC 2 CC9.2:** Vendor risk assessment, monitoring for changes, action on changed vendor risk profile. Broken formula and N/A fields in Gen 1 VDDs mean the output cannot be relied on for CC9.2 evidence.
- **PCI DSS 12.8:** TPSP list, compliance monitoring, PCI obligations in contracts.

---

## Related projects

- `BeckyG-Flex/data-access-automation` — Enforces the downstream provisioning gate once a vendor is approved. This tool (VRM decision tree) is the upstream Security review gate; data-access-automation is the access control gate after approval.
- `~/projects/vendor-risk-management/notes.md` — Full VDD library analysis (43 VDDs), all signal patterns, probing question bank by domain, AI vendor framework, supply chain risk framework, CRIME-208/CPR-117 history, live case pattern map.

---

## Project status

- SEC-2510: In Progress, Sprint 105
- Epic: SEC-2750
- L4 evidence: This deliverable closes the gap Phil named April 8 — Security's role in vendor risk management — and demonstrates conception-to-deployment ownership of a security function where none previously existed.
