# RULES.md - NomMetric Operational Constraints & Guardrails

> **Specification:** OpenGAP Spec 0.1.0  
> **Agent Name:** NomMetric Agent (`nommetric-agent`)  
> **Enforcement Level:** Mandatory & Deterministic  

---

## 1. Attendance & Verification Guardrails

1. **Duplicate Check-In Prohibition**: A student roll number may only be marked present for a single meal session (Breakfast, Lunch, or Dinner) once per calendar date across all participating campus messes.
2. **Meal Window Strictness**: Check-ins are valid only during designated meal operational windows (e.g., Breakfast: 07:30–09:30, Lunch: 12:00–14:00, Dinner: 19:30–21:30) with an immutable ±15 minute administrative grace period.
3. **Multi-Mess Access Isolation**: If a student is registered to Hostels A/B/C with designated dining entitlements, inter-mess guest dining must verify coupon exchange or authorized guest permissions prior to ledger insertion.

---

## 2. Rebate Computation Constraints

1. **Advance Notice Requirement**: Rebate leave applications must be submitted at least 24 hours prior to the first missed meal session to allow dining contractors to adjust raw material procurement.
2. **Consecutive Absence Threshold**: Rebates are credited only when meeting institutional thresholds (e.g., minimum 3 consecutive calendar days / 9 continuous meal sessions).
3. **No Retroactive Rebates**: Unexcused absences cannot be retrospectively converted into refundable rebates without written warden approval.
4. **Cap Protection**: Total monthly rebate deductions cannot exceed the maximum institutionally allowed ceiling (e.g., 50% of the total monthly mess fee).

---

## 3. Data Governance, FERPA & GDPR Standards

1. **PII Masking**: Student roll numbers, email addresses, and dietary preferences must be masked in aggregate reporting and analytics exports.
2. **Zero Commercial Data Sharing**: Dining logs and consumption trends are strictly internal to campus hostel administration and never sold or shared with external advertising third parties.
3. **Read-Only Ledger Enclave**: Past monthly financial statements and finalized attendance logs are locked as read-only snapshots to prevent tampering.

---

## 4. Human-in-the-Loop & Override Rules

1. **Dispute Escalation**: Any contested attendance discrepancy or rebate rejection exceeding \$10 (or INR 500) triggers an automatic escalation ticket to the Hostel Mess Warden.
2. **Manual Override Audit Trail**: Any manual attendance insertion by a mess manager requires a mandatory reason code and digital signature logged in the Firestore audit collection.
