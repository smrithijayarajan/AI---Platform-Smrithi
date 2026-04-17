# Know Me Agent

This folder contains documentation and specifications for the Profile "Know Me" Agent - SAP Concur's universal user intelligence platform service.

## Overview

The Know Me Agent is a platform-level service that maintains comprehensive, context-aware user profiles enabling personalized experiences across all SAP Concur travel and expense products.

**Key goal: Universal user intelligence.**

When any agent in Concur needs to understand a user—whether for booking recommendations, expense validation, policy guidance, or proactive assistance—"Know Me" provides the complete user context.

## Contents

- **Profile-Know-Me-Agent-Requirements.md** - Complete functional and non-functional requirements document

## What Know Me Provides

### Personal & Identity Information
- Contact details (email, phone, address)
- Travel documents (passport, visa, TSA PreCheck)
- Work information (cost center, department, manager)
- Account details (employee ID, permissions)

### Preferences & Behavioral Intelligence
- User preferences (red-eye flights, Marriott loyalty, window seats)
- Behavioral patterns (parks at PAE, flies from SEA, books 2 weeks ahead)
- Relationships (delegates, approvers, frequent travelers)
- Loyalty programs (Marriott Gold, United Premier)
- Context (role, travel frequency, destinations)

### Profile Validation & Completeness
- Detects missing required fields for bookings
- Proactively prompts conversationally to complete profile
- Prevents booking failures by validating completeness
- Updates profile in real-time during conversations


## Core Features

### Feature 1: Missing Profile Field Detection (Priority for Conductor and EA Assistant workstream)
Scenario-based validation that detects missing fields before booking (passport for international, KTN for TSA PreCheck, etc.) and prompts conversationally during booking flow.

### Feature 2: Dynamic Learning & Pattern Recognition ( Priority for EA Assistant workstream)
Automatically learns preferences from booking behavior, expense submissions, and calendar events without manual configuration. Calculates confidence scores and adapts to pattern changes.

## Use Cases

### Booking Agent Use Case
```
Agent Query: "Tell me about Fred Fredericks for booking"

Know Me returns:
- First Class preference, window seat 4A (97% confidence)
- Marriott Bonvoy Platinum Elite (22-year member)
- United MileagePlus Premier Silver
- TSA PreCheck enrolled
- Typical booking patterns and predictions
```

### Expense Agent Use Case
```
Context: Fred submits parking receipt from PAE for SEA flight

Know Me validates:
- Historical pattern: Always parks at PAE (6 past trips)
- Geographic logic: PAE closer to home
- Cost pattern: $15/day typical for PAE
- Recommendation: Auto-approve (89% confidence)
```

### Executive Assistant Use Case
```
Context: Trip extension requires personal life coordination

Know Me provides:
- Spouse: Lisa Fredericks (iMessage preferred)
- Known pattern: Sends flowers when missing plans
- Auto-suggests: Order flowers + notify Lisa
```


## Success Metrics

- **Profile completeness**: 80%+ fields populated
- **Preference accuracy**: % of AI recommendations accepted
- **Booking time reduction**: -50% (target)
- **Expense prediction accuracy**: ±15% of actual spend
- **Auto-approval rate**: Increase due to pattern recognition

## Privacy & Security

- Field-level sensitivity classification
- Scope-based access control
- Purpose-based access logging
- Data masking for sensitive fields
- GDPR compliance (export, deletion, access history)
- Learning opt-out available

## Integration Points

- **Conductor**: Trip extension calendar management
- **Joule**: Conversational profile updates
- **Booking Agent**: Personalized recommendations
- **Expense Agent**: Anomaly detection and validation
- **Executive Assistant Co-Pilot**: Personal context management

## Contact

For questions about the Know Me Agent, contact the Profile Platform team.
