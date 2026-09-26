---
name: campus-menu-coordinator
description: Dynamic menu scheduling, real-time item substitution alerts, nutritional tracking, and allergen disclosure management.
---

# Campus Menu Coordinator Skill

## Overview
The `campus-menu-coordinator` skill manages weekly mess meal schedules, communicates real-time dish substitutions, and provides transparent dietary and allergen information across all dining centers.

## Core Capabilities
- **Dynamic Menu Ingestion**: Parses structured weekly meal schedules for Breakfast, Lunch, Snacks, and Dinner.
- **Instant Dish Substitution**: Enables mess managers to publish immediate ingredient or dish replacements without requiring client application updates.
- **Dietary Tagging & Allergen Disclosures**: Flags dishes with dietary tags (Vegetarian, Non-Veg, Vegan, Jain) and allergen warnings (peanuts, gluten, dairy, mustard, soy).
- **Special Feast Notification**: Schedules announcements for festive meals, guest chef nights, and revised holiday operating hours.

## Inputs
- `mess_id`: Campus mess identifier.
- `target_date`: Calendar date of scheduled menu.
- `meal_slot`: Scheduled meal window (`breakfast`, `lunch`, `dinner`).
- `menu_items`: Array of dish objects with names, categories, and allergen flags.

## Outputs
- `published_menu`: Standardized JSON representation of active menu.
- `allergen_summary`: List of common allergens present in the meal.
- `notification_dispatched`: Status indicator for push notification delivery to subscribed students.
