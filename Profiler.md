
> **Customers don't need to know which regulations or security requirements apply to them. The tool determines the likely requirements from their business, geography, data, customers, contracts and security architecture, and then translates them into migration actions.**


# Migration Compliance & Security Discovery Engine — MVP Proposal

## 1. Executive Summary

During Data Center → Cloud migration, customers frequently discover security, privacy, regulatory, identity or contractual requirements **late in the migration process**.

The problem is not necessarily that customers lack security/compliance teams.

The problem is that:

> **The customer often doesn't know which requirements need to be considered for their particular Atlassian environment.**

Existing compliance questionnaires generally ask customers to provide information such as:

> “Are you GDPR compliant?”
> “Do you require HIPAA?”
> “Do you require data residency?”

This assumes the customer already knows the answer.

The proposed tool reverses the model.

### Current model

```text
Customer knows requirements
          ↓
Customer fills compliance questionnaire
          ↓
Migration team evaluates
```

### Proposed model

```text
Customer describes their environment
          ↓
Discovery questionnaire
          ↓
Rule / decision engine
          ↓
Potential applicable requirements
          ↓
Confidence + reason
          ↓
Migration impact
          ↓
Verification checklist
          ↓
Migration readiness
```

The system therefore becomes an **early-warning system for migration blockers**.

---

# 2. Problem Statement

A typical customer may know:

* They operate in Germany
* They have healthcare customers
* Their security team requires Splunk
* They use Okta
* Some customers require IP allowlisting
* They have government contracts
* They have strict data residency requirements

But they may **not know** that these characteristics could create requirements involving:

* GDPR
* HIPAA
* C5
* data residency
* SIEM
* SSO/SCIM
* contractual security requirements
* government access restrictions
* encryption/key management
* etc.

Therefore the migration team discovers issues only after migration planning has already started.

### Business impact

Late discovery can result in:

* Migration delays
* Rework
* Architecture changes
* Security review delays
* Legal review delays
* Failed customer approvals
* Network redesign
* Identity redesign
* Data residency concerns
* Additional evidence requirements
* Migration plan changes

---

# 3. Product Vision

The product should answer one question:

> **“Based on what you have told us about your organization, what security, privacy, regulatory and governance requirements could affect your migration?”**

The output should **not** simply be a list of laws.

Instead:

```text
Requirement
    ↓
Why it may apply
    ↓
What we need to verify
    ↓
What could change in migration
    ↓
Potential blocker
    ↓
Recommended action
```

---

# 4. Scope / Taxonomy

I recommend using the six-category taxonomy we developed earlier.

## A. Privacy & Data Protection

| ID  | Requirement |
| --- | ----------- |
| A01 | GDPR        |
| A02 | India DPDP  |
| A03 | CCPA/CPRA   |
| A04 | LGPD        |
| A05 | APPI        |
| A06 | PIPL        |
| A07 | PDPA        |
| A08 | PIPA        |
| A09 | POPIA       |
| A10 | DPA / SCC   |

## B. Industry / Regulatory

| ID  | Requirement |
| --- | ----------- |
| B01 | HIPAA       |
| B02 | PCI DSS     |
| B03 | DORA        |
| B04 | NIS2        |
| B05 | FedRAMP     |
| B06 | StateRAMP   |
| B07 | CJIS        |
| B08 | DoD IL      |
| B09 | IRAP        |
| B10 | C5          |
| B11 | TISAX       |

## C. Data & Sovereignty

| ID  | Requirement                                |
| --- | ------------------------------------------ |
| C01 | Data Residency                             |
| C02 | Data Sovereignty                           |
| C03 | Government nationality/access restrictions |
| C04 | Export Control                             |
| C05 | Data Transfer Restrictions                 |

## D. Security Architecture

| ID  | Requirement            |
| --- | ---------------------- |
| D01 | IP Allowlisting        |
| D02 | Egress                 |
| D03 | Private Connectivity   |
| D04 | SIEM                   |
| D05 | Pen Testing            |
| D06 | Vulnerability Scanning |
| D07 | Encryption / CMK / HSM |
| D08 | DLP                    |
| D09 | Data Classification    |

## E. Identity & Administration

| ID  | Requirement             |
| --- | ----------------------- |
| E01 | SSO / SAML              |
| E02 | SCIM                    |
| E03 | RBAC                    |
| E04 | Multi-org               |
| E05 | Tenant Consolidation    |
| E06 | Security Administration |

## F. Governance / Approval

| ID  | Requirement                       |
| --- | --------------------------------- |
| F01 | Legal Review                      |
| F02 | InfoSec Review                    |
| F03 | CAB                               |
| F04 | Security Questionnaire            |
| F05 | Customer Contractual Requirements |

That's **47 assessment targets**.

But importantly, the customer should **not answer 47 questions**.

---

# 5. Questionnaire Design

The MVP should aim for approximately **20–25 primary questions**, with conditional follow-ups.

For example:

```text
Q1 Organization type
     ↓
If Government → Government questions

Q2 Countries
     ↓
If EU → EU questions

Q3 Data types
     ↓
If healthcare data → healthcare questions

Q4 Government customers?
     ↓
If Yes → FedRAMP / CJIS / DoD IL questions
```

This creates a dynamic questionnaire.

---

# 6. Detailed Questionnaire

## Section 1 — Organization

### Q1. What best describes your organization?

**Multi-select**

* Commercial/private organization
* Publicly listed organization
* Government organization
* Government contractor
* Healthcare organization
* Financial services organization
* Automotive organization
* Critical infrastructure organization
* Technology/SaaS provider
* Other
* Not sure

### Signals

```text
Government → FedRAMP / StateRAMP / CJIS / DoD IL
Healthcare → HIPAA
Financial → DORA / PCI
Automotive → TISAX
Critical infrastructure → NIS2
```

---

## Q2. In which countries/regions does your organization operate?

**Multi-select**

* United States
* European Union / EEA
* United Kingdom
* India
* Australia
* Japan
* China
* Singapore
* South Korea
* Brazil
* South Africa
* Canada
* Other
* Not sure

This is a **signal**, not an automatic law determination.

---

# 7. Customer / User Geography

## Q3. Where are your customers, employees or users located?

* US
* EU/EEA
* UK
* India
* Australia
* Japan
* China
* Singapore
* South Korea
* Brazil
* South Africa
* Canada
* Multiple regions
* Not sure

### Why?

This is important because:

```text
Organization location ≠ data subject location
```

For example, a US organization may process EU personal data.

---

# 8. Data Discovery

## Q4. What types of information may be stored in Jira/Confluence or related Atlassian products?

**Multi-select**

* General business information
* Employee information
* Customer personal information
* Names/contact details
* Financial information
* Health/medical information
* Payment/cardholder information
* Government information
* Government-restricted information
* Classified information
* Source code / intellectual property
* Confidential business information
* Security/vulnerability information
* Export-controlled information
* Authentication/identity information
* Other
* Not sure

This is one of the **highest-value questions**.

---

# 9. Personal Data

## Q5. Does the environment contain personal data?

* Yes
* No
* Not sure

If **Yes**:

### Q5a. Whose personal data?

* Employees
* Customers
* Partners
* Vendors
* Public users
* Children/minors
* Multiple categories
* Not sure

### Rule signals

```text
EU + personal data → GDPR candidate
India + personal data → DPDP candidate
California/US + consumer data → CCPA/CPRA candidate
Brazil + personal data → LGPD candidate
Japan + personal data → APPI candidate
China + personal data → PIPL candidate
Singapore/APAC → PDPA candidate
South Korea → PIPA candidate
South Africa → POPIA candidate
```

---

# 10. Regulated Data

## Q6. Does the environment contain any regulated or specially protected data?

* Health information
* Payment/cardholder data
* Financial data
* Government data
* Classified information
* Export-controlled information
* Employee-sensitive information
* None
* Not sure

This question activates several branches.

---

# 11. Healthcare

If health data = Yes:

### Q7. Is the organization subject to healthcare regulations or contracts?

* HIPAA
* HITRUST requirement
* Customer healthcare requirement
* Other healthcare regulation
* Not sure
* No known requirement

### Important rule

Do **not** say:

> Healthcare = HIPAA.

Instead:

```text
Healthcare organization
+
PHI
+
US / covered-entity/business-associate relationship
        ↓
HIPAA = LIKELY
```

The tool should still show **“requires verification.”**

---

# 12. Payment Data

If payment data = Yes:

### Q8. Does the Atlassian environment store, process or transmit cardholder data?

* Yes
* No
* Not sure

### Rule

```text
Cardholder data
        ↓
PCI DSS candidate
        ↓
Determine whether Atlassian environment
is actually within PCI scope
```

Again, don't automatically declare:

> “You are PCI compliant.”

Instead:

> **PCI DSS may affect this environment. Verify whether the Atlassian environment is within your cardholder-data environment/scope.**

---

# 13. Government

### Q9. Do you provide services to government organizations?

* US Federal Government
* US State/Local Government
* Australian Government
* EU Government
* Indian Government
* Other Government
* No
* Not sure

If US Federal:

### Q9a. What type?

* FedRAMP
* DoD
* CJIS
* FISMA/NIST requirement
* StateRAMP
* Agency-specific requirement
* Not sure

---

# 14. Critical Infrastructure

### Q10. Does your organization operate in or support critical/essential infrastructure?

* Yes
* No
* Not sure

If yes:

* Energy
* Transport
* Healthcare
* Banking/financial
* Digital infrastructure
* Water
* Government
* Other

### Rule

```text
EU
+
critical/essential entity
+
covered sector
        ↓
NIS2 = POTENTIAL
```

---

# 15. Financial Services

### Q11. Is your organization a financial entity or does it provide ICT services to financial entities?

* Bank
* Insurance
* Investment
* Payment provider
* Fintech
* Financial-market infrastructure
* ICT provider to financial sector
* Other
* No
* Not sure

### Rule

```text
EU
+
financial sector
        ↓
DORA = POTENTIAL

Explicit DORA requirement
        ↓
DORA = HIGH CONFIDENCE
```

---

# 16. Data Residency

## Q12. Does your organization have a requirement for where Atlassian data must be stored?

* No specific requirement
* EU/EEA
* Germany
* UK
* India
* Australia
* United States
* Japan
* Singapore
* Canada
* Other country
* Multiple countries
* Not sure

### Q12a. Why does the requirement exist?

* Law/regulation
* Government requirement
* Customer contract
* Internal policy
* Security requirement
* Data protection requirement
* Not sure

This distinction is critical.

```text
Residency required because of law
        ≠
Residency required because customer contract says so
```

Both can block migration, but they are different requirement types.

---

# 17. Data Sovereignty

## Q13. Are there restrictions on who can access the data?

* Only personnel in specific countries
* Only nationals of specific countries
* Security-cleared personnel
* Government personnel restrictions
* Foreign government access restrictions
* Jurisdictionally independent infrastructure
* Export-control restrictions
* No restriction
* Not sure

This feeds:

* Data sovereignty
* Government access restrictions
* Export control
* Transfer restrictions

---

# 18. Data Transfers

## Q14. Are there restrictions on transferring data between countries?

* Yes — legal/regulatory
* Yes — customer contract
* Yes — government requirement
* Yes — internal policy
* No
* Not sure

If yes:

> Identify origin countries and destination restrictions.

---

# 19. Network Security

## Q15. Does your organization require special network connectivity to SaaS applications?

* IP allowlisting
* Outbound firewall restrictions
* Proxy
* Secure Web Gateway
* Domain allowlisting
* Private connectivity
* Network segmentation
* No special requirements
* Not sure

### Mapping

```text
IP allowlisting → D01
Egress → D02
Private connectivity → D03
```

---

# 20. Security Monitoring

## Q16. Does your security team require application/security logs to be integrated with a SIEM?

* Yes
* No
* Not sure

If yes:

* Splunk
* Microsoft Sentinel
* QRadar
* Datadog
* Sumo Logic
* Other
* Not sure

Also ask:

> How long must security/audit logs be retained?

* < 30 days
* 30–90 days
* 90–365 days
* > 1 year
* Regulatory requirement
* Not sure

---

# 21. Security Testing

## Q17. Does your organization require SaaS providers to undergo security testing?

* Penetration testing
* Vulnerability scanning
* Independent security assessment
* SIG questionnaire
* CAIQ
* Customer-specific security assessment
* No
* Not sure

Mapping:

```text
Pen test → D05
Vulnerability scanning → D06
Security questionnaire → F04
```

---

# 22. Encryption

## Q18. Does your organization have specific encryption/key-management requirements?

* Standard provider-managed encryption is sufficient
* Customer-managed keys required
* HSM required
* Bring-your-own-key
* Key rotation requirements
* Regulatory encryption requirement
* Customer contractual requirement
* Not sure

Mapping:

```text
CMK → D07
HSM → D07
BYOK → D07
```

---

# 23. DLP / Classification

## Q19. Does your organization classify or monitor sensitive information?

* Data classification
* DLP
* Automated sensitive-data discovery
* Secret scanning
* Source-code scanning
* PII detection
* No
* Not sure

Mapping:

```text
DLP → D08
Classification → D09
```

---

# 24. Identity

## Q20. How are users authenticated?

* Local username/password
* SAML SSO
* Microsoft Entra ID
* Okta
* Ping
* ADFS
* Other identity provider
* Multiple providers
* Not sure

Mapping:

```text
Enterprise SSO → E01
SAML → E01
```

---

# 25. Lifecycle Management

## Q21. How are users provisioned/deprovisioned?

* Manual
* SCIM
* Automated HR-driven provisioning
* Group synchronization
* Just-in-time provisioning
* Automatic deprovisioning
* Other
* Not sure

Mapping:

```text
SCIM → E02
Lifecycle requirement → E02
```

---

# 26. Permissions

## Q22. How complex are your access-control requirements?

* Basic team/project access
* Business-unit separation
* Multiple administrative roles
* Least-privilege requirement
* Separate security administrators
* Highly restricted projects
* Need to preserve complex Data Center permissions
* Multiple organizations/sites
* Not sure

Mapping:

```text
Complex permissions → E03
BU separation → E03
Separate security admin → E06
DC permission preservation → E03
```

---

# 27. Multi-Organization

## Q23. What is your organizational structure?

* Single organization
* Multiple business units
* Multiple Atlassian sites
* Multiple subsidiaries
* Multiple tenants
* Separate customer environments
* Planning consolidation
* Planning separation
* Not sure

Mapping:

```text
Multiple orgs → E04
Tenant consolidation → E05
Security administration → E06
```

---

# 28. Governance

## Q24. Which internal teams must approve SaaS migration?

* Legal
* Privacy
* Information Security
* Security Architecture
* Enterprise Architecture
* Vendor Risk
* CAB
* Compliance
* Procurement
* Business owner
* Customer
* None
* Not sure

Mapping:

```text
Legal → F01
InfoSec → F02
CAB → F03
```

---

# 29. Contractual Requirements

## Q25. Do any customers/contracts impose specific security or compliance requirements?

* Yes
* No
* Not sure

If yes:

* Specific certification
* Specific data residency
* Specific encryption
* Pen testing
* Vulnerability scanning
* Security questionnaire
* SSO
* IP allowlisting
* SIEM
* Incident notification
* Government requirements
* Other
* Not sure

Mapping:

```text
Customer contract
        ↓
F05
        ↓
specific technical requirement
        ↓
D/E/C requirement
```

---

# 30. The Rule Engine

This is the most important part of the product.

I would **not use an LLM as the primary compliance decision-maker**.

Use:

```text
Deterministic rules
        +
Evidence
        +
Confidence
        +
LLM explanation
```

The LLM can explain the result, but the rule engine should produce the result.

---

# 31. Rule Model

Each rule should contain:

```json
{
  "rule_id": "GDPR-001",
  "theme_id": "A01",
  "name": "EU personal data",
  "version": "1.0",
  "conditions": {},
  "result": {},
  "evidence_required": [],
  "migration_impact": {},
  "review": {}
}
```

---

# 32. Example GDPR Rules

### GDPR-001

```text
IF
  personal_data = true
AND
  EU_data_subjects = true

THEN
  GDPR = POTENTIAL
  confidence = 70
```

### GDPR-002

```text
IF
  EU_establishment = true
AND
  personal_data = true

THEN
  GDPR = LIKELY
  confidence = 85
```

### GDPR-003

```text
IF
  customer_contract explicitly requires GDPR/DPA
THEN
  GDPR = HIGH_CONFIDENCE
  confidence = 95
```

### GDPR-004

```text
IF
  personal_data = "unknown"
AND
  EU_users = true

THEN
  GDPR = UNKNOWN
  action = "Determine whether personal data exists"
```

Notice that **unknown is a valid state**.

---

# 33. DPDP Rules

```text
IF
  India_users = true
AND
  personal_data = true

THEN
  DPDP = POTENTIAL
```

Higher confidence:

```text
India operations
+
personal data
+
India data subjects
        ↓
DPDP = LIKELY
```

Explicit contractual/legal requirement:

```text
DPDP requirement explicitly identified
        ↓
HIGH CONFIDENCE
```

---

# 34. HIPAA

```text
IF
  healthcare = true
AND
  PHI = true
AND
  US_healthcare_relationship = true

THEN
  HIPAA = LIKELY
```

Explicit BAA/HIPAA requirement:

```text
HIPAA / BAA requirement = true
        ↓
HIPAA = HIGH CONFIDENCE
```

Healthcare without PHI:

```text
healthcare = true
PHI = false

→ HIPAA = NOT_DETECTED
```

This avoids false positives.

---

# 35. PCI DSS

```text
IF
  cardholder_data = true

THEN
  PCI = POTENTIAL
```

If:

```text
cardholder_data = true
AND
Atlassian_environment_in_CDE = true
```

Then:

```text
PCI = HIGH
migration_impact = HIGH
```

But the tool should ask for scope verification rather than declaring PCI applicability automatically.

---

# 36. DORA

```text
IF
  EU = true
AND
  financial_entity = true

THEN
  DORA = LIKELY
```

Or:

```text
ICT_provider_to_EU_financial_entity = true
AND
DORA_contractual_requirement = true

THEN
  DORA = HIGH_CONFIDENCE
```

---

# 37. NIS2

Conceptually:

```text
IF
  EU = true
AND
  critical_sector = true
AND
  organization_is_covered_entity = true

THEN
  NIS2 = LIKELY
```

If organization isn't sure:

```text
EU
+
critical sector
+
unknown entity classification

→ NIS2 = POTENTIAL
→ verification required
```

---

# 38. FedRAMP

```text
IF
  US_federal_customer = true
AND
  SaaS_environment_supports_federal_workload = true

THEN
  FedRAMP = POTENTIAL
```

Explicit requirement:

```text
customer_contract_requires_FedRAMP = true

→ FedRAMP = HIGH_CONFIDENCE
```

---

# 39. Government / Sovereignty

### Example:

```text
government_customer = true
AND
nationality_restriction = true
```

Result:

```text
C03 Government Access Restrictions
status = LIKELY
impact = HIGH
```

Another:

```text
government_data = true
AND
foreign_access_restriction = true
```

Result:

```text
Data Sovereignty = HIGH
```

---

# 40. Data Residency Rule

```text
IF
  residency_requirement = true

THEN
  C01 = LIKELY
```

Then classify:

```text
driver = LAW
    → regulatory concern

driver = GOVERNMENT
    → sovereignty concern

driver = CONTRACT
    → contractual concern

driver = INTERNAL_POLICY
    → governance/security concern
```

This is a very important design principle.

---

# 41. Network Rules

### IP Allowlisting

```text
IF
  inbound_IP_allowlisting = true

THEN
  D01 = REQUIRED
```

### Egress

```text
IF
  outbound_firewall_restriction = true

THEN
  D02 = REQUIRED
```

### Private connectivity

```text
IF
  private_connectivity_required = true

THEN
  D03 = REQUIRED
  migration_impact = HIGH
```

---

# 42. SIEM

```text
IF
  SIEM_required = true
OR
  security_monitoring = true
OR
  audit_log_retention_required = true

THEN
  D04 = LIKELY
```

If explicit:

```text
SIEM integration is contractual/regulatory
        ↓
D04 = REQUIRED
```

---

# 43. Pen Testing

```text
IF
  pen_testing_required = true

THEN
  D05 = REQUIRED
```

Likewise:

```text
vulnerability_scanning_required = true
        ↓
D06 = REQUIRED
```

---

# 44. Encryption

```text
IF
  CMK_required = true
OR
  HSM_required = true
OR
  BYOK_required = true

THEN
  D07 = HIGH
  migration_impact = HIGH
  blocker = true
```

This is an example where the system should immediately flag:

> ⚠️ **Potential migration blocker**

---

# 45. DLP

```text
IF
  DLP_required = true
OR
  sensitive_data = true
OR
  data_classification_required = true

THEN
  D08 = LIKELY
```

---

# 46. SSO

```text
IF
  enterprise_identity_provider = true
OR
  SAML_required = true

THEN
  E01 = REQUIRED
```

---

# 47. SCIM

```text
IF
  SCIM_required = true
OR
  automatic_deprovisioning_required = true

THEN
  E02 = REQUIRED
```

---

# 48. RBAC

```text
IF
  complex_permissions = true
OR
  least_privilege = true
OR
  business_unit_separation = true
OR
  DC_permission_model_complex = true

THEN
  E03 = LIKELY
```

If migration requires preserving specific permission semantics:

```text
E03 = HIGH
migration_impact = HIGH
```

---

# 49. Multi-org

```text
IF
  multiple_business_units = true
AND
  separate_security_boundaries = true

THEN
  E04 = REQUIRED
```

Tenant consolidation:

```text
multiple_sites = true
AND
consolidation_planned = true

→ E05 = REQUIRED
```

---

# 50. Governance Rules

These aren't laws.

That's important.

If:

```text
InfoSec approval required = true
```

then:

```text
F02 = REQUIRED
```

The report should say:

> **Internal approval dependency**

not:

> “You are subject to an InfoSec regulation.”

Similarly:

```text
CAB = true
```

→ migration governance dependency.

---

# 51. Confidence Model

I recommend five states.

| Status       | Meaning                             |
| ------------ | ----------------------------------- |
| NOT_DETECTED | No evidence found                   |
| POTENTIAL    | Some signals indicate applicability |
| LIKELY       | Strong evidence                     |
| REQUIRED     | Explicit requirement identified     |
| UNKNOWN      | Insufficient information            |

And separately:

```text
confidence: 0–100
```

Example:

```json
{
  "theme": "GDPR",
  "status": "LIKELY",
  "confidence": 84
}
```

---

# 52. Don't Just Use Confidence

A requirement can have:

```text
Confidence = 95
Impact = Low
```

or:

```text
Confidence = 65
Impact = High
```

The second may actually be more important to the migration team.

Therefore calculate:

### Migration Risk

```text
Migration Risk =
    Applicability Confidence
    ×
    Migration Impact
    ×
    Unresolved Evidence
```

This gives you a much more useful prioritization model.

---

# 53. Example Final Result

Suppose the customer answers:

```text
Organization:
Financial services

Geography:
EU + India

Data:
Customer personal data
Financial information

Identity:
Okta + SAML + SCIM

Security:
Splunk
Pen testing

Residency:
EU

Customers:
Enterprise + government
```

The output might be:

| Requirement             | Status       | Impact |
| ----------------------- | ------------ | ------ |
| GDPR                    | 🟠 Likely    | High   |
| DPDP                    | 🟠 Potential | Medium |
| DORA                    | 🔴 Likely    | High   |
| EU Residency            | 🔴 Required  | High   |
| SSO                     | 🔴 Required  | Medium |
| SCIM                    | 🔴 Required  | Medium |
| SIEM                    | 🔴 Required  | High   |
| Pen Testing             | 🟠 Likely    | Medium |
| RBAC                    | 🟠 Likely    | High   |
| Government restrictions | 🟡 Verify    | High   |

---

# 54. The Most Important UX Feature

For every result, show three layers:

### Customer said

> “We require EU data residency.”

### System inferred

> “EU data residency may affect your Cloud migration because the destination must satisfy your stated geographic restriction.”

### Still to verify

> “Confirm whether the requirement originates from regulation, customer contract or internal policy.”

This builds trust.

---

# 55. Evidence Model

Every rule should generate an evidence request.

Example:

```text
GDPR
 ├── Evidence
 │   ├── Data inventory
 │   ├── Data subject locations
 │   ├── DPA
 │   └── Transfer mechanism
 │
 └── Migration checks
     ├── Cloud data residency
     ├── Data flows
     ├── Subprocessors
     └── Transfer requirements
```

---

# 56. JSON Output

The rule engine could return:

```json
{
  "assessment_id": "ASSESS-001",
  "profile": {
    "regions": ["EU", "India"],
    "industries": ["Financial Services"],
    "data_types": [
      "personal_data",
      "financial_data"
    ]
  },
  "results": [
    {
      "theme_id": "A01",
      "name": "GDPR",
      "status": "LIKELY",
      "confidence": 84,
      "migration_impact": "HIGH",
      "potential_blocker": true,
      "signals": [
        "EU data subjects",
        "personal data"
      ],
      "verification": [
        "Confirm data subject locations",
        "Review DPA requirements",
        "Confirm international transfer requirements"
      ]
    },
    {
      "theme_id": "D04",
      "name": "SIEM",
      "status": "REQUIRED",
      "confidence": 96,
      "migration_impact": "HIGH",
      "potential_blocker": true,
      "signals": [
        "SIEM integration required",
        "Splunk used"
      ],
      "verification": [
        "Confirm audit log requirements",
        "Confirm retention period",
        "Confirm SIEM integration requirements"
      ]
    }
  ]
}
```

---

# 57. Architecture

For MVP, keep the architecture relatively simple.

```text
                  ┌──────────────────────┐
                  │   Customer UI        │
                  │ Dynamic Questionnaire│
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Profile Normalizer   │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Deterministic Rule   │
                  │ Engine               │
                  └──────────┬───────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
          Privacy        Regulatory      Security
           Rules            Rules          Rules
              │              │              │
              └──────────────┼──────────────┘
                             ▼
                  ┌──────────────────────┐
                  │ Assessment Engine    │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Risk / Prioritization│
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ AI Explanation Layer │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Migration Readiness  │
                  │ Dashboard            │
                  └──────────────────────┘
```

---

# 58. Where AI Should Be Used

I would deliberately **not make this an “AI compliance agent.”**

Use AI for:

### 1. Explanation

Turn:

```text
GDPR = 84
signals = EU + personal data
```

into:

> GDPR may apply because your organization processes personal data associated with individuals in the EU. Confirm the applicable processing activities and transfer requirements before migration.

### 2. Follow-up questions

AI can identify:

> “You said you have government customers but haven't specified which country.”

Then dynamically ask a follow-up.

### 3. Evidence interpretation

Customer uploads:

* Security policy
* Data classification policy
* DPA
* customer security questionnaire
* contract

AI can extract relevant requirements.

### 4. Historical issue learning

This is potentially the most powerful future capability.

```text
Historical migration issue
       ↓
AI classification
       ↓
Theme
       ↓
Existing rule
       ↓
Rule gap
       ↓
New candidate rule
```

But humans should approve rule changes.

---

# 59. Rule Engine Should Be Versioned

Do **not** hard-code everything in Java/TypeScript.

Store rules as data.

Example:

```text
/rules
   /privacy
      gdpr.json
      dpdp.json
      ccpa.json

   /regulatory
      hipaa.json
      dora.json
      nis2.json

   /sovereignty
      residency.json
      sovereignty.json

   /security
      siem.json
      network.json
      encryption.json

   /identity
      sso.json
      scim.json
      rbac.json

   /governance
      legal.json
      infosec.json
```

Each rule:

```json
{
  "rule_id": "GDPR-001",
  "version": "1.2",
  "effective_from": "2026-01-01",
  "review_required": true,
  "conditions": [],
  "output": {},
  "evidence": []
}
```

This becomes very important as regulations and Atlassian capabilities evolve.

---

# 60. MVP Dashboard

I would have five main sections.

## 1. Overall Readiness

```text
Migration Readiness

████████░░ 72%

🔴 5 High-risk items
🟠 8 Items requiring verification
🟢 14 Low-risk items
```

---

## 2. Potential Blockers

Example:

```text
🔴 EU Data Residency
   Confidence: 94%
   Impact: HIGH

   Why:
   Customer requires EU-only data storage.

   Action:
   Confirm Cloud residency requirements.

────────────────────────

🔴 SIEM
   Confidence: 91%
   Impact: HIGH

   Why:
   Customer requires Splunk integration.

   Action:
   Validate audit-log export requirements.
```

---

# 61. Requirement Explorer

Users can navigate:

```text
Privacy
 ├── GDPR
 ├── DPDP
 └── CCPA

Regulatory
 ├── DORA
 ├── HIPAA
 └── PCI DSS

Data
 ├── Residency
 ├── Sovereignty
 └── Transfers

Security
 ├── SIEM
 ├── Network
 └── Encryption

Identity
 ├── SSO
 ├── SCIM
 └── RBAC

Governance
 ├── Legal
 ├── InfoSec
 └── CAB
```

---

# 62. Migration Action Plan

This is where the product becomes more valuable than a generic compliance checker.

Instead of:

> “GDPR applies.”

Say:

```text
Before Migration
──────────────────────────────

1. Confirm EU residency requirement
   Owner: Data Protection

2. Confirm DPA requirements
   Owner: Legal

3. Validate data transfer requirements
   Owner: Privacy

4. Validate Cloud data residency
   Owner: Migration team

5. Validate audit/security controls
   Owner: InfoSec
```

---

# 63. MVP Phases

## Phase 1 — Rule Engine Prototype

Build:

* 47 assessment targets
* ~25 questions
* conditional questions
* normalized customer profile
* deterministic rules
* confidence
* evidence
* migration impact

Start with the **highest-value themes**, rather than implementing every regulation deeply.

### Priority 1

* GDPR
* DPDP
* HIPAA
* PCI DSS
* DORA
* FedRAMP
* Data Residency
* Data Sovereignty
* IP Allowlisting
* SIEM
* Encryption
* SSO
* SCIM
* RBAC
* Legal/InfoSec/Contractual

---

# 64. Phase 2 — Historical Validation

This is extremely important.

Take your historical migration issues:

```text
Issue #1
"Customer requires SIEM"
       ↓
D04

Issue #2
"Customer requires Germany residency"
       ↓
C01

Issue #3
"Customer cannot use public network"
       ↓
D03

Issue #4
"Customer requires complex identity lifecycle"
       ↓
E02
```

Then calculate:

```text
Historical issues detected
──────────────────────────
Rule coverage: 82%

False positives: 7%

False negatives: 11%
```

Then improve the questionnaire/rules.

**This should be the validation mechanism for the MVP.**

---

# 65. Phase 3 — Document Intelligence

Allow customers to upload:

* security policies
* compliance questionnaires
* contracts
* DPAs
* architecture documents
* data classification documents

AI extracts:

```text
"Customer requires IP allowlisting"

→ D01

"Data must remain within Germany"

→ C01

"Annual penetration testing required"

→ D05
```

The extracted statement becomes **evidence**, not an unquestioned rule.

---

# 66. Phase 4 — Migration Integration

Eventually integrate actual Data Center information.

For example:

```text
Customer Questionnaire
        +
Jira DC Configuration
        +
Confluence configuration
        +
Users
        +
Groups
        +
Projects
        +
Filters
        +
Boards
        +
Network information
        ↓
Migration Compliance Assessment
```

This is where the product becomes much more differentiated.

The system can say:

> Customer requires SCIM.

But then inspect the DC environment and say:

> **1,842 users and 73 groups detected. Validate identity migration strategy.**

That's much more actionable.

---

# 67. Phase 5 — Continuous Migration Risk

Eventually the assessment should not be a one-time questionnaire.

```text
Initial assessment
       ↓
Migration planning
       ↓
Configuration discovery
       ↓
Migration changes
       ↓
Re-assessment
       ↓
Readiness
```

The product becomes a **Migration Compliance Control Plane**.

---

# 68. What Makes This Different From TrustScan

A privacy checker primarily answers:

> **Which privacy laws might apply?**

Your product answers:

> **What could block my Atlassian Cloud migration, and what do I need to verify before I migrate?**

That's a much more specific problem.

### TrustScan-style model

```text
Country
Industry
Data
   ↓
Privacy laws
```

### Your model

```text
Organization
+
Geography
+
Industry
+
Data
+
Customers
+
Government
+
Contracts
+
Security
+
Identity
+
Network
+
Governance
        ↓
Compliance
+
Security
+
Architecture
+
Identity
+
Governance
        ↓
Migration blockers
        ↓
Actions
```

---

# 69. Most Important MVP Principle

I would make this a **decision-support tool**, not a legal advice engine.

The wording should therefore be:

> **Potentially applicable**

> **Likely applicable**

> **Requires verification**

> **Migration impact**

rather than:

> “You are legally required to…”

That distinction protects the product from pretending that a questionnaire can replace legal/compliance advice.

---

# 70. The MVP in One Sentence

If I had to put the entire proposal into one product statement:

> **A rules-driven migration readiness engine that discovers hidden security, privacy, regulatory, identity, data sovereignty and governance requirements from a customer's environment—without requiring the customer to already know which regulations apply—and converts them into prioritized migration actions and potential blockers.**

### The next thing I would build

Rather than immediately coding the questionnaire, I would create a **master rule matrix** with all 47 targets in this format:

| Rule ID   | Theme     | Trigger Questions | Conditions         | Result    | Confidence | Impact | Blocker | Evidence              | Verification              | Action               |
| --------- | --------- | ----------------- | ------------------ | --------- | ---------: | ------ | ------- | --------------------- | ------------------------- | -------------------- |
| GDPR-001  | GDPR      | Q3,Q5             | EU + personal data | Likely    |         84 | High   | Yes     | Data inventory        | Verify EU data subjects   | Review DPA/residency |
| HIPAA-001 | HIPAA     | Q1,Q6,Q7          | Healthcare + PHI   | Likely    |         80 | High   | Yes     | PHI classification    | Verify HIPAA scope        | Review BAA           |
| DORA-001  | DORA      | Q1,Q11            | EU + financial     | Potential |         65 | High   | Yes     | Entity classification | Verify DORA applicability | Security review      |
| RES-001   | Residency | Q12               | Residency required | Required  |         95 | High   | Yes     | Contract/policy       | Verify target region      | Residency validation |
| SIEM-001  | SIEM      | Q16               | SIEM required      | Required  |         95 | High   | Yes     | Security policy       | Validate logging          | SIEM assessment      |
| SSO-001   | SSO       | Q20               | SAML/IdP           | Required  |         95 | Medium | No      | IAM policy            | Validate federation       | Identity assessment  |

**That matrix becomes the actual backbone of the MVP.** From it, we can automatically derive the questionnaire, JSON rule files, API contract, dashboard and test cases.
