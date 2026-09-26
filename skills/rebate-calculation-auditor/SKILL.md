---
name: rebate-calculation-auditor
description: Deterministic evaluation of hostel leave applications, consecutive meal absence thresholds, and fair rebate fee calculations.
---

# Rebate Calculation Auditor Skill

## Overview
The `rebate-calculation-auditor` skill automates the assessment of student mess leave requests and executes mathematically deterministic rebate deductions in full compliance with hostel administration regulations.

## Core Capabilities
- **Advance Notice Auditing**: Validates that leave applications are received at least 24 hours prior to the first affected meal session.
- **Continuous Absence Thresholding**: Checks that leave durations satisfy institutional thresholds (e.g., minimum 3 consecutive calendar days / 9 continuous meals).
- **Pro-Rata Meal Credit Computation**: Computes exact financial refunds using per-meal weighted formulas ($w_b = 0.20, w_l = 0.40, w_d = 0.40$).
- **Ceiling & Cap Enforcement**: Ensures total monthly rebate deductions do not exceed the institutional threshold ceiling (maximum 50% of the monthly base fee).
- **Warden Appeal Routing**: Flags border-line cases or medical emergencies for manual administrative approval.

## Inputs
- `student_id`: Student identification number.
- `departure_date`: ISO 8601 start timestamp of absence.
- `return_date`: ISO 8601 conclusion timestamp of absence.
- `base_monthly_fee`: Monetary base cost of monthly mess membership.
- `absence_reason`: Categorical reason (Academic, Medical, Vacation).

## Outputs
- `is_eligible`: Boolean eligibility indicator.
- `total_missed_meals`: Breakdown of missed breakfasts, lunches, and dinners.
- `rebate_amount`: Total currency deduction credited to the student's next billing cycle.
- `explanation_code`: Explicit rationale or refusal justification.
