# Joule Mobile User Experience

## Scenario: Fred's Trip Extension

### Notification Screen

```
📱 SAP Concur - Joule
─────────────────────────────────────
Your SF trip extended through tomorrow.
Return flight: 7:30 PM (leave hotel 4:30 PM)

📅 TOMORROW'S SCHEDULE:

✅ 9:00 AM - Dorian @ Salesforce Tower
   Location: San Francisco ✓ No conflict
   [No action needed]

✅ 10:30 AM - Team Sync
   Virtual meeting ✓ No conflict
   [No action needed]

⚠️ 2:00 PM - Budget Review with Sarah
   Location: Reston Conference Room
   You'll be in San Francisco

   Choose action:
   ○ Request virtual (Sarah's availability: Open)
   ○ Move to Fri 3/21 2:00 PM (Sarah: Free)
   ○ Decline meeting

⚠️ 4:00 PM - 1:1 with Manager (James)
   Location: James's office, Reston
   ⏰ CONFLICTS with airport departure (4:30 PM)

   Choose action:
   ○ Request virtual (James's availability: Open)
   ○ Move to Fri 3/21 3:00 PM (James: Free) [RECOMMENDED]
   ○ Decline meeting

───────────────────────────────────

QUICK OPTIONS:
[Request virtual for all]
[Move all afternoon meetings to Friday]
[Let me choose individually] ← Selected

[Review my choices] [Cancel]
```

---

### After Fred Makes Selections

```
📱 SAP Concur - Joule
─────────────────────────────────────
✓ Your selections:

2:00 PM Budget Review
→ Request virtual (email to Sarah)

4:00 PM 1:1 with James
→ Move to Friday 3 PM (reschedule request)

📧 Draft emails ready:

Email 1 to Sarah:
"Hi Sarah, my SF trip extended - can we do
Budget Review virtually at 2 PM tomorrow?
I'll join from SF. Thanks, Fred"

Email 2 to James:
"Hi James, my SF trip extended and I have a
7:30 PM flight tomorrow. Can we move our 1:1
to Friday at 3 PM instead? You're showing as
free at that time. Thanks, Fred"

[Edit emails] [Send both] [Cancel]
```

---

### Bulk Action: Move All Afternoon Meetings

```
📱 If Fred taps "Move all afternoon meetings to Friday"
─────────────────────────────────────
I'll move all meetings after 2 PM to Friday:

✓ 2:00 PM Budget Review → Fri 3/21 2:00 PM
  (Sarah is free)

✓ 4:00 PM 1:1 with James → Fri 3/21 3:00 PM
  (James is free)

This clears your afternoon for airport travel.

Emails will explain: "SF trip extended, need to
catch evening flight, proposing Friday instead."

[Confirm & send] [Adjust times] [Cancel]
```

---

### After Emails Sent

```
📱 SAP Concur - Joule
─────────────────────────────────────
✓ Emails sent:

✓ Sarah (Budget Review) - Requested virtual
✓ James (1:1) - Proposed Friday 3 PM reschedule

I'll monitor for responses and notify you
when they reply.

[View sent emails] [Done]
```

---

### When Organizer Responds

```
📱 SAP Concur - Joule
─────────────────────────────────────
✅ Sarah replied to Budget Review request:

"No problem, I'll update the meeting to Teams.
See you at 2 PM tomorrow."

📅 Updated calendar invite received:
   Budget Review - now showing Teams link

1 conflict resolved.
Still waiting for James to respond to 1:1 reschedule.

[View updated meeting] [OK]
```

---

### If No Response After 2 Hours

```
📱 SAP Concur - Joule
─────────────────────────────────────
⏰ James hasn't responded to your 1:1
reschedule request yet (sent 2 hours ago).

Your flight leaves at 7:30 PM tomorrow, so
the 4 PM meeting still conflicts.

Options:
├─ Send follow-up reminder to James
├─ Call James directly (click to dial)
├─ Decline the 4 PM meeting with explanation
└─ Keep as-is (I'll remind you at 3 PM tomorrow)

What should I do?

[Send reminder] [Call James] [Decline] [Wait]
```

---

## Design Principles

### 1. **Progressive Disclosure**
- Show high-level conflicts first
- Expand to detailed options when user taps "Let me choose individually"
- Hide complexity for users who want quick bulk actions

### 2. **Context-Aware Recommendations**
- Mark time conflicts as higher priority (airport departure)
- Show organizer availability inline ("Sarah: Free")
- Recommend best option based on meeting history and constraints

### 3. **Transparent AI Actions**
- Show draft emails before sending
- Allow editing of auto-generated messages
- Confirm all actions before executing

### 4. **Closed-Loop Communication**
- Monitor for organizer responses
- Notify when conflicts resolved
- Escalate if no response within time window

### 5. **Flexibility**
- Provide both bulk actions and granular control
- Allow users to switch between modes
- Support "I'll handle it manually" escape hatch

---

## User Testing Scenarios

### Scenario 1: Power User (Wants Control)
- Selects "Let me choose individually"
- Reviews each meeting's options
- Edits draft emails before sending
- **Result**: Full control, 2 minutes total time

### Scenario 2: Busy Executive (Wants Speed)
- Taps "Move all afternoon meetings to Friday"
- Reviews bulk action summary
- Taps "Confirm & send"
- **Result**: Zero cognitive load, 15 seconds total time

### Scenario 3: Cautious User (Wants to Review)
- Lets Calendar Agent detect conflicts
- Receives notification
- Chooses to handle manually
- **Result**: Awareness of conflicts, zero automated actions

---

## Success Metrics

- **Time to resolution**: < 1 minute (vs. 15-20 minutes manual)
- **User satisfaction**: 85+ NPS score
- **Adoption rate**: 70%+ of business travelers using feature within 6 months
- **Email accuracy**: 95%+ of auto-generated emails sent without edits
- **Conflict resolution rate**: 90%+ of conflicts resolved before departure
