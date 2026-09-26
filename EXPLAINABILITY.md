# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **NomMetric Agent** (`nommetric-agent`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** NomMetric Agent (`nommetric-agent`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Education / Campus Logistics & Dining Facility Orchestration  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), FERPA, GDPR  

---

## How the Agent Decides

NomMetric Agent is an autonomous campus mess tracking, student meal attendance logging, rebate calculation auditing, and multi-mess dining analytics orchestration agent designed for **NomMetric** (OpenCode '25). The agent coordinates attendance validation, rebate eligibility calculations, dynamic menu updates, and kitchen demand forecasting across campus dining halls.

### 1. Decision Architecture

The dining management, attendance logging, and rebate auditing lifecycle operates across a deterministic, five-stage pipeline:

```
Student / Administrator Event (Meal Check-In / Rebate Application / Menu Update / Monthly Billing)
    │
    ▼
[Stage 1: Identity & Entitlement Verification]
    │  - Authenticates student identity via roll number, token, or dynamic QR code
    │  - Validates active hostel enrollment and mess registration status in Firestore
    │  - Checks for active disciplinary or financial holds
    ▼
[Stage 2: Session Window & Concurrency Guard]
    │  - Evaluates current timestamp against active meal window:
    │      ├── Breakfast (07:30 - 09:30)
    │      ├── Lunch (12:00 - 14:00)
    │      └── Dinner (19:30 - 21:30)
    │  - Queries meal attendance ledger to prevent duplicate check-ins across multiple messes
    │  - Flags concurrent multi-facility entry attempts
    ▼
[Stage 3: Rebate Policy & Eligibility Engine]
    │  - For Leave / Rebate Applications:
    │      ├── Verifies advance notice window ($\ge 24\text{ hours}$ before first departure meal)
    │      ├── Computes continuous absence interval: $D_{\text{absence}} = T_{\text{return}} - T_{\text{departure}}$
    │      └── Enforces institutional minimum threshold: $D_{\text{absence}} \ge 3\text{ consecutive days}$
    │  - Calculates daily meal refund allowance: $R_{\text{meal}} = \frac{\text{Monthly Mess Base Fee}}{\text{Days in Month} \times 3}$
    ▼
[Stage 4: Kitchen Demand & Analytics Aggregation]
    │  - Tallies real-time attendance counts and active leave rosters per dining facility
    │  - Computes predictive turnout forecast: $N_{\text{expected}} = N_{\text{registered}} - N_{\text{rebate\_approved}} - N_{\text{historical\_noshow}}$
    │  - Broadcasts prep adjustments to kitchen supervisors to curb food wastage
    ▼
[Stage 5: Immutable Ledger Commit & Audit Logging]
    │  - Persists atomic check-in transaction in Firestore with ISO 8601 timestamp
    │  - Generates cryptographic verification hash for tamper-evident recordkeeping
    │  - Emits real-time state change event to student mobile application via Riverpod
    ▼
Final Reconciled Dining Ledger & Monthly Mess Fee Deduction
```

### 2. Decision Logic & Rebate Computation Formula

The rebate calculation engine evaluates student financial adjustments with strict mathematical determinism:

1. **Daily Meal Rate Calculation**:
   $$R_{\text{day}} = \frac{F_{\text{base}}}{D_{\text{month}}}$$
   where $F_{\text{base}}$ is the mandatory monthly mess fee and $D_{\text{month}}$ is the number of days in the operational billing cycle.

2. **Per-Session Meal Weighting**:
   - Breakfast: $w_b = 0.20$ (20% of daily fee)
   - Lunch: $w_l = 0.40$ (40% of daily fee)
   - Dinner: $w_d = 0.40$ (40% of daily fee)
   $$\sum w_i = 1.0$$

3. **Total Rebate Credit**:
   $$R_{\text{total}} = \sum_{m \in M_{\text{eligible}}} (R_{\text{day}} \times w_m)$$
   subject to the maximum rebate ceiling constraint:
   $$R_{\text{total}} \le C_{\text{max}} \times F_{\text{base}} \quad (\text{where } C_{\text{max}} = 0.50)$$

### 3. Thresholding & Refusal Decision Criteria

NomMetric Agent deterministically refuses operations under explicit boundary conditions:
- **Refusal on Duplicate Check-In**: If a student roll number has already checked into any mess for the current meal session, subsequent check-in requests are refused with code `ERR_DUPLICATE_SESSION_ENTRY`.
- **Refusal on Late Rebate Application**: Applications submitted under 24 hours prior to departure are rejected with code `ERR_INSUFFICIENT_ADVANCE_NOTICE`.
- **Refusal on Sub-Threshold Leave**: Requests for leaves shorter than 3 consecutive calendar days (or fewer than 9 consecutive meals) are rejected with code `ERR_MINIMUM_ABSENCE_NOT_MET`.
- **Refusal on Tampered QR Code**: QR check-in payloads that fail cryptographic timestamp expiration ($>60\text{ seconds}$) are rejected with code `ERR_EXPIRED_QR_TOKEN`.

### 4. Fallback Decision Mechanism

NomMetric Agent incorporates resilient multi-tier fallbacks to support campus operational continuity:
- **Offline Ledger Caching Fallback**: If campus Wi-Fi or cellular connectivity drops during peak dinner rush, the Flutter client switches to SQLite offline check-in caching with cryptographic queue sequencing, syncing transactions atomically to Firestore upon network restoration.
- **Manual Roll-Number Fallback**: If camera scanner hardware malfunctions, the agent permits authenticated mess supervisors to perform manual student roll-number entries with compulsory biometric/supervisor PIN verification.
- **Model Fallback Cascade**: Automated menu parsing, dietary advisory, and analytics summary tasks default to `gemini-2.0-flash` with seamless failover to `gpt-4o` and `claude-3-5-sonnet`.

### 5. Human-in-the-Loop Governance

NomMetric preserves administrative oversight and student advocacy across all sensitive operations:
- **Hostel Warden Review for Medical Exemptions**: Students who fail the 24-hour advance notice window due to emergency illness can submit medical certificates for warden override and retroactive rebate approval.
- **Mess Committee Menu Oversight**: AI-generated menu suggestions and nutrition-balanced rotations require explicit approval from the student-elected Hostel Mess Committee.
- **Dispute Resolution Protocol**: Any student can challenge an attendance omission or billing anomaly via an in-app dispute workflow routed directly to the Chief Warden.

---

## The Data It Uses

NomMetric Agent adheres to strict educational data privacy standards, operating under zero-telemetry and open-source compliance standards.

### 1. Ingested Input Data

The agent processes only verified campus dining data points:
- **Student Dining Profiles**: Anonymized student identifier, assigned hostel block, mess membership type, and registered meal plan.
- **Attendance Check-In Payloads**: Session timestamp, mess facility ID, device verification hash, and meal category (Breakfast, Lunch, Dinner).
- **Leave & Rebate Submissions**: Leave start date, expected return date, reason classification (Academic Conference, Vacation, Medical), and supporting documentation.
- **Mess Menu Offerings**: Weekly food rosters, dish descriptions, dietary tags (Vegetarian, Non-Vegetarian, Jain, Vegan), and major allergen disclosures.

### 2. Configuration & Reference Data

- **Institutional Mess Calendar**: Academic term dates, scheduled semester breaks, and official holiday dining timetables.
- **Hostel Rebate Policy Schema**: Configuration YAML defining notice cutoff hours, minimum absence thresholds, daily fee allocations, and maximum rebate deduction caps.
- **Allergen Reference Tables**: Standardized food allergy classifications based on international food safety guidelines.

### 3. Base Model & Inference Lineage

- **Deterministic Accounting & Validation Linters**: All attendance verification, concurrency checks, and rebate computations are executed using deterministic, rule-based algorithmic procedures (100% reproducible with zero LLM variance).
- **AI Analytics & Communication Copilot**: High-capability foundation models (`gemini-2.0-flash`, `gpt-4o`, `claude-3-5-sonnet`) utilized exclusively for natural-language weekly dining digests, demand forecast summaries, and dietary assistance.
- **Zero Training on Student Data**: Student dining logs, attendance histories, and personal identifiers are never stored on external third-party servers or used for public foundation model fine-tuning.

### 4. Data Privacy, Storage, and Retention

- **FERPA & GDPR Compliance**: In full accordance with FERPA and GDPR (Articles 5, 17, and 28), all student dining records are classified as sensitive educational records with encrypted rest storage and role-based access control.
- **Automatic Data Lifecycle & Purging**: Detailed meal check-in logs are retained for the active academic semester to support audit reconciliations, after which individual records are pruned into anonymized aggregate analytics.
- **Zero Commercial Monetization**: Student dietary patterns, dining hours, and mess preferences are strictly quarantined and never sold to third-party advertisers.

---

## Limitations

Understanding the operational boundaries and constraints of NomMetric Agent is critical for maintaining reliable campus mess management.

### 1. Offline Synchronization Latency & Concurrency Windows
- **Limitation**: When multiple mess facilities operate offline concurrently during connectivity disruptions, a student could conceivably attempt duplicate check-ins at two different messes before local SQLite queues sync with central Firestore.
- **Mitigation**: Local queues record high-resolution monotonically increasing hardware timestamps; upon reconnection, the earliest timestamp is acknowledged, and subsequent duplicate records are automatically flagged for warden administrative review.

### 2. Dynamic Unscheduled Menu Substitutions
- **Limitation**: Campus kitchens occasionally encounter last-minute supplier shortages, forcing unannounced ingredient or dish substitutions that are not immediately reflected in the digital menu.
- **Mitigation**: The agent provides mess managers with a quick 1-tap "Substitute Item" mobile widget to broadcast instant menu change alerts to student devices.

### 3. Hardware Scanner & Camera Focus Variance
- **Limitation**: Damaged student ID barcodes, scratched phone screens, or low-light conditions at mess entry doors can cause QR code optical scanner read failures.
- **Mitigation**: The system supports secure numeric roll-number lookup with two-factor supervisor PIN verification to prevent entry queues from bottlenecking.

### 4. Edge-Case Absence Window Misalignments
- **Limitation**: Student travel schedules may involve fractional departure days (e.g., departing campus after breakfast but before lunch), creating ambiguity in daily rate rebate calculations.
- **Mitigation**: The rebate calculation engine breaks each calendar day into explicit weighted meal slots ($w_b = 0.20, w_l = 0.40, w_d = 0.40$), calculating refunds on a per-meal rather than whole-day basis.

### 5. Subjective Meal Quality & Sensory Feedback
- **Limitation**: While the agent tracks portion counts, nutritional estimates, and turnouts, it cannot objectively assess culinary taste, seasoning, or temperature.
- **Mitigation**: NomMetric integrates a structured 5-star student feedback rating channel with specific tags (taste, hygiene, temperature, portion) reviewed weekly by the student mess committee.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & rebate computation formula | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & warden oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested student profiles, check-ins, leaves & menus | Section 1 | Verified |
| - Configuration, academic calendar & rebate schemas | Section 2 | Verified |
| - Base model lineage & deterministic calculations | Section 3 | Verified |
| - Data privacy, retention lifecycle & FERPA/GDPR | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Offline sync latency & concurrency windows | Section 1 | Verified |
| - Dynamic unscheduled menu substitutions | Section 2 | Verified |
| - Hardware scanner & camera focus variance | Section 3 | Verified |
| - Edge-case fractional day absence misalignments | Section 4 | Verified |
| - Subjective meal quality & sensory feedback | Section 5 | Verified |
