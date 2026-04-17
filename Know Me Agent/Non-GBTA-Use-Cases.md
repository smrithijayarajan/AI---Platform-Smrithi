# Non-GBTA Use Cases for Know Me Agent

This document contains Know Me Agent use cases that are **not** part of the GBTA (Global Business Travel Association) conference demo roadmap but are valuable for improving user experience and data quality across SAP Concur products.

---

## Use Case 1: TextApp Mobile Number Verification

### Context

From integration meeting discussion between Smrithi and James: Embedding the profile agent at key points in the user journey, such as the travel information page or expense page, to prompt for mobile number verification when relevant, reducing friction and improving user experience.

### Problem Statement

Many users have incomplete or unverified mobile phone numbers in their profiles, which:
- Prevents them from receiving SMS notifications for travel updates
- Blocks TextApp functionality for expense receipt capture
- Results in missed flight delay/cancellation alerts
- Reduces effectiveness of duty of care communications

Currently, users only discover they need to verify their mobile number when they attempt to use a feature that requires it, creating friction at the worst possible moment (e.g., during booking or trying to submit an expense).

---

## User Story 1: Proactive Mobile Number Prompt During Booking

**As a** traveler booking a trip
**I want to** be prompted to verify my mobile number when it's missing or unverified
**So that** I can receive flight alerts and use SMS-based features without friction later

### Acceptance Criteria

- Know Me Agent detects missing/unverified mobile number during booking flow
- System prompts conversationally at appropriate moment (after destination selected, before flight selection)
- Explains value: "Add your mobile number to receive flight delay alerts and text receipts from your trip"
- Allows user to add/verify number inline without leaving booking flow
- User can skip if desired ("Remind me later" option)
- Once verified, enables TextApp and SMS notifications automatically

### User Flow

```
User: "Book me a flight to San Francisco"

Booking Agent: "Great! I'm searching flights to San Francisco..."

[Know Me Agent detects: Mobile number missing]

Booking Agent: "Before I show you flight options, I noticed your mobile
number isn't in your profile. Adding it now will let you:
• Receive SMS alerts if your flight is delayed
• Text photos of receipts during your trip (no need to save paper)
• Get gate change notifications

What's your mobile number? (You can skip this if you prefer)"

User: "+1 703 555 0142"

Booking Agent: "Got it! I've sent a verification code to +1 (703) 555-0142.
Can you share the 6-digit code?"

User: "749283"

Booking Agent: "✓ Mobile number verified and saved to your profile.
You're all set for SMS alerts and TextApp.


