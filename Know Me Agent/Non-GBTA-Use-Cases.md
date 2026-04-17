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

Now, here are your flight options to San Francisco..."
```

### Technical Flow

```
1. User initiates booking
   ↓
2. Booking Agent queries Know Me: "Does user have verified mobile?"
   ↓
3. Know Me Response: { mobileNumber: null, verified: false }
   ↓
4. Booking Agent: Conversational prompt (with skip option)
   ↓
5. User provides mobile number
   ↓
6. Know Me Agent: Send SMS verification code via Twilio
   ↓
7. User provides verification code
   ↓
8. Know Me Agent: Verify code + update profile
   ↓
9. Know Me Agent: Enable TextApp feature flag
   ↓
10. Booking Agent: Continue with flight selection
```

### API Integration

#### Check Mobile Verification Status
```
GET /know-me/v1/users/{userId}/validation?context=booking

Response:
{
  "userId": "fred.fredericks@sap.com",
  "mobileNumber": {
    "present": false,
    "verified": false,
    "required": false,
    "recommended": true,
    "reason": "Enables SMS alerts and TextApp receipt capture"
  }
}
```

#### Request Verification Code
```
POST /know-me/v1/users/{userId}/mobile/verify/send

Request:
{
  "mobileNumber": "+17035550142",
  "channel": "sms",
  "purpose": "Enable SMS notifications and TextApp"
}

Response:
{
  "verificationId": "ver_abc123",
  "codeSentTo": "+1 (703) 555-0142",
  "expiresIn": 600,
  "message": "Verification code sent via SMS"
}
```

#### Verify Code and Update Profile
```
POST /know-me/v1/users/{userId}/mobile/verify/confirm

Request:
{
  "verificationId": "ver_abc123",
  "code": "749283"
}

Response:
{
  "verified": true,
  "mobileNumber": "+17035550142",
  "features enabled": ["sms_notifications", "textapp", "duty_of_care"],
  "profileUpdated": true
}
```

---

## User Story 2: TextApp Detects Missing Mobile and Agent Updates Profile

**As a** traveler who wants to text my receipts using TextApp
**I want to** be prompted for my mobile number when it's missing from my profile
**So that** the Know Me Agent can update it for me and enable TextApp immediately

### Acceptance Criteria

- User attempts to use TextApp feature (e.g., asks "I want to text my receipts")
- Know Me Agent detects missing mobile number
- Agent prompts conversationally: "I need your mobile number to enable TextApp"
- User provides mobile number in conversation
- Know Me Agent automatically verifies and updates profile
- User receives confirmation with TextApp phone number
- No manual navigation to profile settings required

### User Flow

```
User: "I want to text my receipts"

Joule/TextApp Agent: "Great! TextApp lets you text photos of receipts
directly to Concur. Let me set that up for you."

[Know Me Agent detects: Mobile number missing from profile]

Joule: "I need your mobile number to enable TextApp. What number should
I use?"

User: "+1 703 555 0142"

Joule: "Perfect! I've sent a verification code to +1 (703) 555-0142.
Can you share the 6-digit code you received?"

User: "749283"

[Know Me Agent verifies code and updates profile automatically]

Joule: "✓ Done! Your mobile number is verified and TextApp is enabled.

Text photos of your receipts to: +1 (888) 260-2905

I've updated your profile so you're all set for future trips too."
```

### Technical Flow

```
1. User requests TextApp feature
   ↓
2. TextApp Agent queries Know Me: "Can user use TextApp?"
   ↓
3. Know Me Response: { textAppEnabled: false, reason: "mobileNumber missing" }
   ↓
4. TextApp Agent: Prompt user for mobile number
   ↓
5. User provides mobile number
   ↓
6. Know Me Agent: Send SMS verification code
   ↓
7. User provides verification code
   ↓
8. Know Me Agent: Verify code
   ↓
9. Know Me Agent: Automatically update profile
   - Write mobileNumber to Identity Service
   - Set mobileVerified = true
   - Enable TextApp feature flag
   ↓
10. TextApp Agent: Confirm and provide TextApp number
```

---

## User Story 3: Travel Information Page Mobile Verification

**As a** traveler on the travel information page
**I want to** verify my mobile number to receive flight alerts
**So that** I don't miss important travel notifications

### Acceptance Criteria

- Know Me Agent detects unverified mobile when user views upcoming trip
- Shows contextual prompt: "Get SMS alerts for this trip"
- Explains specific value for their upcoming travel
- Inline verification without leaving page
- After verification, confirms SMS alerts are enabled for upcoming trip
- Updates profile for all future trips

### User Flow

```
[User views upcoming trip: San Francisco, Apr 25-27]

Contextual card appears:
┌────────────────────────────────────────────────────┐
│ ✈️ Get SMS alerts for your San Francisco trip      │
│                                                     │
│ Verify your mobile number to receive:              │
│ • Flight delay/cancellation alerts                 │
│ • Gate change notifications                        │
│ • Boarding reminders                               │
│                                                     │
│ [Verify Mobile Number]  [Not Now]                  │
└────────────────────────────────────────────────────┘

[User clicks "Verify Mobile Number"]

Quick verification flow:
1. Enter mobile number (or confirm if already present)
2. Receive SMS code
3. Enter code
4. ✓ Verified

Confirmation message:
┌────────────────────────────────────────────────────┐
│ ✓ Mobile number verified!                          │
│                                                     │
│ You'll receive SMS alerts for:                     │
│ • United Flight UA 1523 to SFO (Apr 25)            │
│ • United Flight UA 2247 from SFO (Apr 27)          │
│                                                     │
│ SMS alerts are now enabled for all your trips.     │
└────────────────────────────────────────────────────┘
```

---

## User Story 4: Post-Booking Mobile Number Suggestion

**As a** traveler who just completed a booking
**I want to** be offered the option to add my mobile number
**So that** I receive alerts for the trip I just booked

### Acceptance Criteria

- Know Me Agent detects booking completion without verified mobile
- Shows success message with mobile number prompt
- Contextual to the trip just booked
- Non-intrusive (appears after booking confirmation)
- One-click defer: "Maybe later"
- Remembers if user declined (don't ask again for 30 days)

### User Flow

```
[User completes booking]

Booking confirmation page shows:
┌────────────────────────────────────────────────────┐
│ ✓ Your trip to San Francisco is confirmed!         │
│                                                     │
│ Confirmation: ABC123                                │
│ Flight: United UA 1523, Apr 25 at 8:00 AM         │
│ Hotel: Marriott Union Square, 3 nights             │
└────────────────────────────────────────────────────┘

Additional card appears:
┌────────────────────────────────────────────────────┐
│ 💡 One more thing — Stay updated on your trip      │
│                                                     │
│ Add your mobile number to get SMS alerts if your   │
│ flight is delayed or your gate changes.            │
│                                                     │
│ [Add Mobile Number]  [Maybe Later]                 │
└────────────────────────────────────────────────────┘
```

---

## Benefits by User Journey Stage

### During Booking
- **Context**: User is actively planning travel
- **Value**: "Enable notifications for this trip"
- **Timing**: After destination selected, before flight selection
- **Success rate**: High (user is engaged, sees immediate value)

### On Expense Page
- **Context**: User needs to submit receipts
- **Value**: "Skip manual uploads, text receipts instead"
- **Timing**: First visit to expense page without TextApp
- **Success rate**: Medium (some users prefer manual upload)

### On Travel Info Page
- **Context**: User viewing upcoming trip
- **Value**: "Get alerts for your upcoming flight"
- **Timing**: When viewing trip details 1-7 days before departure
- **Success rate**: High (imminent travel = high motivation)

### Post-Booking
- **Context**: User just completed booking
- **Value**: "Stay updated on the trip you just booked"
- **Timing**: Immediately after booking confirmation
- **Success rate**: Medium (user may be in hurry to complete booking)

---

## Integration Points

### Know Me Agent Responsibilities
1. **Detection**: Identify missing/unverified mobile numbers
2. **Verification**: Send SMS codes via Twilio/SMS provider
3. **Validation**: Verify codes and update profile
4. **Feature Enablement**: Activate TextApp, SMS notifications
5. **Persistence**: Store verified number in Identity Service
6. **Audit Trail**: Log verification events

### Booking Agent Integration
- Query Know Me for mobile verification status
- Display conversational prompts at appropriate moment
- Pass user input to Know Me for verification
- Continue booking flow after verification (or skip)

### Expense Page Integration
- Query Know Me for TextApp status on page load
- Show banner if TextApp not enabled
- Inline verification form
- Display TextApp phone number after verification

### Travel Information Page Integration
- Query Know Me for SMS notification status
- Show contextual card for upcoming trips
- Inline verification flow
- Confirmation of alerts enabled for specific flights

---

## Technical Requirements

### SMS Provider Integration
- **Provider**: Twilio (or equivalent)
- **Rate limiting**: Max 3 verification attempts per hour per user
- **Code expiration**: 10 minutes
- **Code format**: 6-digit numeric
- **Retry**: Allow resend after 60 seconds

### Profile Storage
- **Field**: `mobileNumber` (E.164 format: +17035550142)
- **Verification status**: `mobileVerified` (boolean + timestamp)
- **Features enabled**: `textAppEnabled`, `smsNotificationsEnabled`
- **Verification history**: Audit log of verification events

### Feature Flags
- **TextApp**: Automatically enabled after mobile verification
- **SMS Notifications**: Enabled for flight alerts, gate changes, delays
- **Duty of Care**: Enabled for emergency communications

---

## Success Metrics

- **Verification rate**: % of users who complete mobile verification when prompted
- **Drop-off points**: Where users abandon verification flow
- **TextApp adoption**: % increase in TextApp usage after implementing prompts
- **SMS notification engagement**: % of users who open/act on SMS alerts
- **Profile completeness**: % of users with verified mobile numbers
- **Time to verify**: Average time from prompt to completed verification

---

## Privacy & Security

### User Consent
- Clear explanation of why mobile number is requested
- Explicit consent before sending verification SMS
- Option to skip/decline at every step
- Ability to remove mobile number from profile later

### Data Protection
- Mobile numbers stored encrypted at rest
- Transmitted over TLS
- Masked in UI (show last 4 digits: +1 (XXX) XXX-0142)
- GDPR-compliant (right to deletion)

### Verification Security
- Rate limiting on verification attempts
- Code expiration (10 minutes)
- One-time use codes
- Audit trail of verification events
- Block suspicious patterns (multiple numbers, rapid retries)

---

## Future Enhancements

### Multi-Channel Verification
- Voice call option for verification (accessibility)
- WhatsApp verification for international users
- Email fallback if SMS fails

### Smart Prompting
- ML-based timing: Prompt when user is most likely to complete verification
- Context-aware: Different prompts for business vs. leisure travel
- Suppress prompts: Don't ask again if user declined 3+ times

### International Support
- Country-specific phone number formats
- International SMS providers
- Local language verification messages

---

## Related Documentation

- [Know Me Agent Main Requirements](./Profile-Know-Me-Agent-Requirements.md)
- [EA Assistant Integration](./EA-Assistant-Know-Me-Requirements.md)
- [TextApp Feature Documentation](https://confluence.concur.com/display/TEXTAPP)
- [Twilio Integration Guide](https://confluence.concur.com/display/PLATFORM/Twilio)
