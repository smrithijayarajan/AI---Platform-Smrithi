# Preferences and Behavioral Intelligence

This folder contains documentation for travel preferences and behavioral learning capabilities of the Know Me Agent.

## Overview

The Know Me Agent learns user preferences automatically from booking behavior, expense submissions, and calendar events—enabling personalized, intelligent recommendations without manual configuration.

## Contents

- **Preference-Fields-and-Learning.md** - Complete documentation of preference fields and behavioral intelligence:
  - Travel Preferences (flights, hotels, cars, rail)
  - Rate Preferences (AAA, AARP, government, military discounts)
  - Rail-Specific Preferences (BahnCard, seat types, quiet zones)
  - Hotel Amenities (gym, pool, accessibility, pillows)
  - Car Rental Add-Ons (GPS, ski rack)
  - System Preferences (language, eReceipts)
  - Behavioral Learning (pattern detection, confidence scoring, adaptive recommendations)

## Key Capabilities

### Automatic Preference Learning
- **Flight patterns**: Seat position (window/aisle), time of day, airline loyalty
- **Hotel patterns**: Brand loyalty, room floor, specific properties
- **Ground transport**: Service provider (Lyft/Uber), upgrade tier, timing
- **Dining patterns**: Cuisine types, solo vs. group, budget levels
- **Expense patterns**: Parking locations, receipt timing, spending context

### Confidence Scoring
```
Confidence = (Frequency × 40%) + (Consistency × 40%) + (Recency × 20%)
```
- High (>90%): Auto-apply preferences
- Medium (70-90%): Suggest but don't auto-apply
- Low (<70%): Neutral, don't recommend

### Adaptive Intelligence
- Detects pattern shifts (e.g., switches from window to aisle seats)
- Prompts user to confirm preference changes
- Adjusts confidence scores based on new behavior

## Use Cases

### Booking Agent
- Pre-fills departure airport, auto-selects preferred seat (window 4A)
- Prioritizes Marriott hotels in search results (97% confidence)
- Applies BahnCard discount for German rail bookings

### Expense Agent
- Validates unusual patterns (parks at PAE for SEA flights—learned exception)
- Pre-fills estimated costs based on historical averages
- Flags anomalies (e.g., $200 parking vs. typical $45)

### EA Assistant
- Surfaces traveler preferences to arrangers (first class, Marriott Platinum Elite)
- Predicts trip costs before booking based on patterns
- Recommends dining reservations at 7pm (Fred's preferred time)

## Related Documentation

- [Know Me Agent Main Requirements](../Profile-Know-Me-Agent-Requirements.md)
- [Profile Validation](../Profile%20Validation/) - Required/Recommended fields for bookings
- [EA Assistant Integration](../EA-Assistant-Know-Me-Requirements.md)
