# Calendar + Outlook Agent Use Case for Conductor

## Use Case: Trip Extension + Flexible Meeting Management

**1. Trigger:** Fred's San Francisco trip gets extended by 1 day with return flight tomorrow at 7:30 PM (must leave hotel by 4:30 PM for airport).

**2. Problem:** Fred has 4 meetings tomorrow: 2 in morning (can attend from SF), 2 in afternoon (one conflicts with Reston location, one conflicts with airport departure time) - needs flexible options to manage each meeting individually or in bulk.

**3. Detection:** Calendar Agent scans tomorrow's schedule, maps meetings against Fred's location (SF not Reston) and travel timeline (must leave by 4:30 PM), flags 2 PM Budget Review (location conflict) and 4 PM 1:1 (location + time conflict with departure).

**4. Intelligence:** Outlook Agent analyzes each conflicting meeting: checks email history for virtual precedent, reads organizers' free/busy calendars to find reschedule options (e.g., "Sarah free Friday 2 PM, James free Friday 3 PM"), determines meeting flexibility (recurring vs. critical one-time), and calculates travel buffer requirements.

**5. User Choice:** Joule presents Fred with per-meeting options (Request virtual / Move to specific day+time / Decline) plus bulk actions ("Move all afternoon meetings to Friday" or "Request virtual for all"), showing organizer availability for proposed times and recommending best option based on context (e.g., 4 PM meeting should move due to airport conflict).

**6. Execution:** Outlook Agent sends customized emails based on Fred's choices: virtual requests include trip extension context, reschedule requests include proposed time with confirmation that organizer is free, bulk moves include explanation about flight departure, then monitors responses and notifies Fred when organizers accept/update.

**7. Outcome:** Fred manages 4 meetings in 45 seconds (vs. 20+ minutes manually), can choose individual actions or bulk operations based on preference, afternoon cleared for airport travel with no scheduling conflicts, and all organizers receive professional communication with proposed alternatives and availability pre-checked.

---

## Key Ask from Calendar PM

**Read-only Graph API access to:**
- Detect when user's travel dates change (via TripIt/Concur itinerary integration)
- Read user's calendar events (date, time, location, attendees, organizer)
- Read organizer free/busy calendars for reschedule suggestions
- Identify location-based conflicts (meeting location ≠ user's actual location)
- Identify time-based conflicts (meeting time conflicts with travel departure)

**Outlook Send API access to:**
- Send emails on user's behalf (after approval)
- Monitor inbox for organizer responses

**Target: August 2026 MVP**

---

## Business Value

- **Time Savings**: 15-20 minutes per trip extension event
- **User Experience**: Mobile-first, one-tap approval workflow
- **Missed Meetings**: Zero (automatic conflict detection and resolution)
- **Professional Communication**: Context-aware emails sent automatically
- **Flexibility**: Individual meeting control or bulk actions based on user preference
