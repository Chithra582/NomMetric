---
name: meal-attendance-tracker
description: Real-time verification, session window management, and duplicate check-in prevention for campus mess dining halls.
---

# Meal Attendance Tracker Skill

## Overview
The `meal-attendance-tracker` skill validates student meal check-ins in real-time across multiple campus dining facilities, enforcing operational meal windows, multi-mess isolation, and duplicate attendance prevention.

## Core Capabilities
- **Session Validation**: Verifies check-in requests against active meal operational windows (Breakfast, Lunch, Dinner).
- **Concurrency & Duplicate Guard**: Ensures that a student cannot be recorded as dining at multiple campus messes during the same meal session.
- **Offline Ledger Queueing**: Handles network dropouts gracefully by queueing timestamped attendance records locally in SQLite with tamper-evident checksums before synchronization to Firestore.
- **Grace Period Enforcement**: Applies configurable administrative grace periods (±15 minutes) for student arrival spikes.

## Inputs
- `student_id`: Unique student roll number or cryptographic identification token.
- `mess_id`: Campus dining hall identifier (e.g., `MESS_BOYS_HOSTEL_1`, `MESS_GIRLS_HOSTEL_2`).
- `meal_session`: Enumerated meal type (`breakfast`, `lunch`, `dinner`).
- `timestamp`: ISO 8601 formatted event timestamp.

## Outputs
- `status`: Verification outcome (`GRANTED`, `REJECTED_DUPLICATE`, `REJECTED_OUTSIDE_WINDOW`, `REJECTED_UNAUTHORIZED`).
- `attendance_id`: Unique ledger transaction identifier.
- `mess_headcount`: Updated real-time attendance count for the active facility.
