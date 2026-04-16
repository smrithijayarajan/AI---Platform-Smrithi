# Calendar Agent for Conductor

This folder contains documentation and specifications for the Calendar + Outlook Agent integration with SAP Concur Conductor.

## Overview

The Calendar Agent is a core component of Conductor that detects travel schedule changes and automatically manages calendar conflicts. When paired with the Outlook Agent, it provides intelligent meeting management during trip extensions.

## Contents

- **Use-Case-7-Liner.md** - Executive summary of the trip extension use case for Calendar PM stakeholders
- **Detailed-Use-Case.md** - Complete technical specification with user flows
- **Joule-Mobile-UX.md** - Mobile interface mockups and user experience design

## Use Case Summary

When a business traveler's trip gets extended (e.g., additional meeting added), the Calendar + Outlook Agents automatically:

1. Detect calendar conflicts (meetings scheduled at wrong location)
2. Analyze meeting flexibility (virtual precedent, organizer availability)
3. Present smart options (request virtual / reschedule / decline)
4. Send professional emails on user's behalf (after approval)
5. Monitor responses and notify when conflicts resolved

## Target Launch

**August 2026 MVP**

## Key Dependencies

- Microsoft Graph API (Calendar read access)
- Microsoft Graph API (Free/Busy read access)
- Outlook Send Mail API
- Concur TripIt integration (travel itinerary detection)
- SAP Concur Conductor orchestration platform
- Joule mobile interface

## Contact

For questions about this use case, contact the Profile Platform team.
