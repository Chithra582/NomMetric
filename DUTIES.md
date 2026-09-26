# DUTIES.md - NomMetric Operational Responsibilities & Workflows

> **Specification:** OpenGAP Spec 0.1.0  
> **Agent Name:** NomMetric Agent (`nommetric-agent`)  
> **Lifecycle Stages:** Daily Operations, Monthly Reconciliation, Anomaly Handling, Audit Reporting  

---

## 1. Daily Meal & Attendance Orchestration

- **Session Initialization**: Activate meal verification services 15 minutes before the start of each meal window (Breakfast, Lunch, Dinner).
- **QR / Roll Number Check-In Processing**: Validate student QR codes or roll numbers against the active mess registration registry in Firestore.
- **Real-Time Headcount Monitoring**: Maintain live headcount meters per mess facility to notify kitchen supervisors of dining flow rates and remaining capacity.
- **Session Closure & Snapshotting**: Lock meal session ledgers exactly at window close, compile total attendance metrics, and flag missed meals for rebate evaluation.

---

## 2. Rebate Eligibility & Leave Application Auditing

- **Application Ingestion**: Review incoming student leave applications, validating departure and return dates against academic calendar allowances.
- **Advance Notice Check**: Enforce the institutional advance notification window (minimum 24-hour cutoff before meal departure).
- **Rule Verification**: Compute contiguous missed meal counts and evaluate against the institutional minimum threshold (e.g., $\ge 3$ consecutive days).
- **Ledger Staging**: Mark upcoming meal slots as "Approved Rebate" in the student ledger, preventing double-billing while disallowing check-ins during the leave duration.

---

## 3. Menu Coordination & Dietary Scheduling

- **Weekly Menu Ingestion**: Parse and structure daily meal offerings (Main course, sides, desserts, beverages) from the hostel mess committee.
- **Dynamic Updates**: Support real-time menu substitutions without requiring mobile application redeployment or cache invalidation.
- **Dietary & Allergen Tagging**: Tag menu items with nutritional information and standard allergen indicators (dairy, nuts, gluten, spice intensity).
- **Special Feast Notification**: Broadcast notifications for festival feasts, special banquets, and holiday operational timings.

---

## 4. Monthly Financial Reconciliation & Reporting

- **Consolidated Billing Compilation**: Compute each student's net monthly mess billing:
  $$\text{Net Mess Dues} = \text{Base Monthly Fee} - \text{Total Approved Rebates} + \text{Guest Meal Surcharges}$$
- **Warden Review Dossier**: Generate executive summary reports for hostel wardens highlighting total meals served, total rebate credits issued, and uncollected dues.
- **Audit Logging**: Commit monthly reconciliation records to the immutable audit collection with cryptographic checksums.
