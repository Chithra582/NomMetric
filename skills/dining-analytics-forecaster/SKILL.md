---
name: dining-analytics-forecaster
description: Campus dining headcount forecasting, food waste analytics, monthly consumption patterns, and hostel warden dossier generation.
---

# Dining Analytics Forecaster Skill

## Overview
The `dining-analytics-forecaster` skill aggregates multi-mess attendance history, active rebate leaves, and day-of-week consumption patterns to generate predictive meal headcount forecasts and institutional dining audits.

## Core Capabilities
- **Predictive Turnout Estimation**: Calculates expected dining turnout per meal using registered rosters minus approved leaves minus historical no-show factors.
- **Food Waste Mitigation**: Informs kitchen contractors of anticipated meal volumes 4 hours prior to cooking, curtailing over-preparation and ingredient spoilage.
- **Monthly Warden Dossiers**: Aggregates total meals served, total rebate credits issued, and billing reconciliation summaries for university administration.
- **Consumption Trend Discovery**: Identifies student meal preferences, unpopular recipes, and seasonal dining variations.

## Inputs
- `mess_id`: Campus mess facility identifier.
- `target_session`: Date and meal slot to forecast.
- `historical_days_sample`: Rolling window size (e.g., past 30 days) for no-show baseline estimation.

## Outputs
- `expected_diners`: Statistically projected diner headcount.
- `procurement_guideline`: Recommended ingredient scaling factor ($0.80 - 1.10$).
- `confidence_interval`: Variance range for expected headcount.
