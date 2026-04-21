# Executive Assistant Use Case: Multi-Traveler International Booking

## Scenario: AI Workshop in Bellevue - 3 Employees

**Context:** Executive Assistant Sarah needs to book travel to Bellevue, WA (USA) for an AI workshop. Three employees with different origins and requirements:
- **Priya Sharma** - Traveling from Bangalore, India (Indian citizen)
- **Marc Dubois** - Traveling from Vancouver, Canada (Canadian citizen)
- **Elena Müller** - Traveling from Munich, Germany (German citizen)

**Workshop Dates:** May 15-17, 2026

---

## Know Me Agent Workflow

### Step 1: EA Co-Pilot Initiates Booking Request

```
EA (Sarah): "I need to book travel to Bellevue for Priya Sharma, Marc Dubois,
and Elena Müller for the AI workshop May 15-17."

EA Co-Pilot: "Got it! I'm checking their profiles to validate travel requirements
and preferences. Let me scan for any issues before we start booking..."

[EA Co-Pilot queries Know Me Agent for all 3 travelers]
```

---

### Step 2: Know Me Agent Validates Each Traveler

#### **Traveler 1: Priya Sharma (India → Bellevue, USA)**

**Know Me Profile Check:**
```
GET /know-me/v1/users/priya.sharma@sap.com/validation?context=international&destination=US&origin=IN

Response:
{
  "travelerId": "priya.sharma@sap.com",
  "profileCompleteness": "68%",
  "blockingIssues": [
    {
      "field": "visaNumber",
      "severity": "CRITICAL",
      "reason": "US B1/B2 visa required for Indian citizens entering USA",
      "impact": "Cannot board flight without valid US visa. Airline will deny boarding.",
      "action": "Obtain visa before booking or verify existing visa"
    }
  ],
  "warnings": [
    {
      "field": "passportExpiration",
      "severity": "WARNING",
      "value": "2026-08-30",
      "reason": "Passport expires in 4 months. USA requires 6-month validity beyond travel dates.",
      "impact": "May be denied entry at US border. Airline may deny boarding.",
      "action": "Renew passport before travel"
    }
  ],
  "missingRecommended": [
    {
      "field": "primaryEmail",
      "severity": "CRITICAL_NON_BLOCKING",
      "impact": "Cannot receive boarding pass, flight alerts, or itinerary updates"
    },
    {
      "field": "primaryMobilePhone",
      "severity": "CRITICAL_NON_BLOCKING",
      "impact": "Cannot receive SMS gate changes (critical for international connections)"
    },
    {
      "field": "tsaPrecheck",
      "severity": "RECOMMENDED",
      "impact": "Extra 15-30 min security wait time in USA"
    }
  ],
  "preferences": {
    "flight": {
      "seatPosition": "window",
      "confidence": "92%",
      "mealPreference": "vegetarian",
      "confidence": "98%"
    },
    "hotel": {
      "brandLoyalty": "Marriott Bonvoy Gold (India)",
      "roomType": "king",
      "confidence": "85%"
    }
  }
}
```

#### **Traveler 2: Marc Dubois (Vancouver → Bellevue, USA)**

**Know Me Profile Check:**
```
GET /know-me/v1/users/marc.dubois@sap.com/validation?context=international&destination=US&origin=CA

Response:
{
  "travelerId": "marc.dubois@sap.com",
  "profileCompleteness": "95%",
  "blockingIssues": [],
  "warnings": [],
  "missingRecommended": [
    {
      "field": "nexusNumber",
      "severity": "RECOMMENDED",
      "reason": "NEXUS enables fast border crossing Canada-USA",
      "impact": "Saves 45+ minutes at border/airport security. Only $50 for 5 years.",
      "savings": "30-60 min per trip"
    }
  ],
  "preferences": {
    "flight": {
      "homeAirport": "YVR",
      "seatPosition": "aisle",
      "confidence": "96%",
      "airline": "Alaska Airlines (frequent flyer AS123456)",
      "confidence": "88%"
    },
    "hotel": {
      "brandLoyalty": "Hilton Honors Diamond (12 years)",
      "roomType": "king, high floor",
      "confidence": "94%",
      "amenities": ["gym", "early check-in"]
    },
    "ground": {
      "provider": "Uber",
      "tier": "Uber Black (prefers premium)",
      "confidence": "79%"
    }
  }
}
```

#### **Traveler 3: Elena Müller (Munich → Bellevue, USA)**

**Know Me Profile Check:**
```
GET /know-me/v1/users/elena.mueller@sap.com/validation?context=international&destination=US&origin=DE

Response:
{
  "travelerId": "elena.mueller@sap.com",
  "profileCompleteness": "78%",
  "blockingIssues": [],
  "warnings": [
    {
      "field": "estaAuthorization",
      "severity": "CRITICAL_NON_BLOCKING",
      "reason": "German citizens require ESTA for visa-waiver travel to USA",
      "impact": "Cannot board flight without ESTA approval (processing takes 72 hours)",
      "action": "Apply for ESTA immediately if not already obtained",
      "cost": "$21 USD",
      "url": "https://esta.cbp.dhs.gov"
    }
  ],
  "missingRecommended": [
    {
      "field": "primaryMobilePhone",
      "severity": "CRITICAL_NON_BLOCKING",
      "impact": "Cannot receive SMS for international flight delays/gate changes"
    }
  ],
  "preferences": {
    "flight": {
      "homeAirport": "MUC",
      "seatPosition": "window",
      "section": "forward",
      "confidence": "91%",
      "mealPreference": "gluten-free",
      "confidence": "100%",
      "airline": "Lufthansa (Senator status, LH567890)",
      "confidence": "94%"
    },
    "hotel": {
      "brandLoyalty": "Marriott Bonvoy Platinum",
      "roomType": "suite or king high floor",
      "confidence": "89%",
      "amenities": ["gym", "pool", "restaurant"]
    },
    "rail": {
      "bahnCard": "BahnCard 50 First Class",
      "confidence": "100%"
    }
  },
    "patterns": {
      "businessClass": "85% of long-haul flights (>6 hours)",
      "loungeAccess": "Always uses Senator lounge MUC"
    }
  }
}
```

---

### Step 3: EA Co-Pilot Surfaces Validation Issues

```
EA Co-Pilot: "⚠️ I've found some critical issues that need attention before booking:

**Priya Sharma (India → USA):**
🚨 BLOCKING: No US visa on file
   → Action: Priya needs to apply for US B1/B2 visa (processing: 2-4 weeks)
   → OR: Verify if she already has valid visa not in profile

⚠️ WARNING: Passport expires Aug 30, 2026
   → Only 4 months validity (USA requires 6+ months)
   → Risk of entry denial. Recommend passport renewal ASAP.

📧 CRITICAL: Missing email address
   → Cannot send boarding pass or flight updates

📱 CRITICAL: Missing mobile phone
   → Cannot send SMS gate changes (critical for BLR-SEA-BLR connections)

**Marc Dubois (Vancouver → USA):**
✅ Profile complete - ready to book!
💡 Recommendation: Add NEXUS number (saves 45+ min at border)

**Elena Müller (Munich → USA):**
⚠️ CRITICAL: ESTA authorization not verified
   → German citizens need ESTA for USA travel ($21, 72-hour processing)
   → Action: Apply immediately at https://esta.cbp.dhs.gov

📱 CRITICAL: Missing mobile phone
   → Cannot receive international flight alerts

---

**Next Steps:**
1. ❌ CANNOT book Priya until US visa verified
2. ❌ CANNOT book Elena until ESTA verified
3. ✅ CAN book Marc immediately

Should I:
A) Reach out to Priya and Elena to complete missing requirements?
B) Book Marc now and wait on the others?
C) Send all 3 a profile completion request?"
```

---

### Step 4: EA Co-Pilot Generates Work Items for Travel Arranger

```
EA (Sarah): "Generate work items for Priya and Elena. Book Marc now using his preferences."

EA Co-Pilot: "Perfect! I'm creating work items on your dashboard now..."

[Know Me Agent generates work items for EA dashboard]
```

#### **Work Item Dashboard for Sarah (Travel Arranger)**

```
┌─────────────────────────────────────────────────────────────────────┐
│  EA CO-PILOT DASHBOARD - Work Items for Sarah                      │
│  Bellevue AI Workshop Trip (May 15-17, 2026)                       │
└─────────────────────────────────────────────────────────────────────┘

🚨 CRITICAL - BLOCKING BOOKINGS (2 travelers)

┌─────────────────────────────────────────────────────────────────────┐
│ Priya Sharma (India → USA)                                         │
│ Priority: URGENT | Status: WAITING ON TRAVELER                     │
│                                                                     │
│ 🚨 BLOCKING ISSUES:                                                │
│ • US Visa Not Verified                                             │
│   Reason: Indian citizens require B1/B2 visa for USA entry         │
│   Impact: Cannot book flight without valid visa                    │
│   Processing Time: 2-4 weeks                                       │
│                                                                     │
│   Actions for Sarah:                                               │
│   [ ] Contact Priya to verify if she has existing US visa         │
│   [ ] If yes: Request visa number, issue date, expiration         │
│   [ ] If no: Inform Priya to apply immediately                    │
│       → https://ustraveldocs.com/in/                               │
│   [ ] Update profile once visa confirmed                           │
│                                                                     │
│ ⚠️ WARNING:                                                         │
│ • Passport Expires: Aug 30, 2026 (4 months)                       │
│   Risk: USA requires 6+ months validity beyond travel dates       │
│   Recommendation: Advise Priya to begin passport renewal           │
│                                                                     │
│ 📧 CRITICAL - Missing Contact Info:                                │
│ • Email: Not on file (cannot send boarding pass/updates)          │
│ • Mobile: Not on file (cannot send SMS gate change alerts)        │
│                                                                     │
│   Actions for Sarah:                                               │
│   [ ] Request Priya's email and mobile number                     │
│   [ ] Update profile with contact information                     │
│                                                                     │
│ ✈️ Booking Ready Once Complete:                                    │
│    Window seat, vegetarian meal, Marriott Gold                    │
│                                                                     │
│ [Contact Priya] [Update Profile] [Mark as Complete]               │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│ Elena Müller (Germany → USA)                                       │
│ Priority: HIGH | Status: WAITING ON TRAVELER                       │
│                                                                     │
│ ⚠️ CRITICAL - BLOCKING ISSUE:                                      │
│ • ESTA Authorization Not Verified                                  │
│   Reason: German citizens need ESTA for visa-waiver travel to USA │
│   Cost: $21 USD                                                    │
│   Processing: Up to 72 hours                                       │
│   Impact: Cannot board flight without ESTA approval                │
│                                                                     │
│   Actions for Sarah:                                               │
│   [ ] Contact Elena to check if ESTA already obtained             │
│   [ ] If no: Provide ESTA application link                        │
│       → https://esta.cbp.dhs.gov                                   │
│   [ ] Once approved: Request ESTA number from Elena               │
│   [ ] Update profile with ESTA authorization                       │
│                                                                     │
│ 📱 CRITICAL - Missing Contact Info:                                │
│ • Mobile: Not on file (cannot send international flight alerts)   │
│                                                                     │
│   Actions for Sarah:                                               │
│   [ ] Request Elena's mobile number                               │
│   [ ] Update profile for SMS notifications                        │
│                                                                     │
│ ✈️ Booking Ready Once Complete:                                    │
│    Lufthansa business class, window forward, gluten-free meal     │
│    Marriott Platinum suite                                        │
│                                                                     │
│ Timeline: Apply within 24 hours to book by Apr 24                 │
│                                                                     │
│ [Contact Elena] [Update Profile] [Mark as Complete]               │
└─────────────────────────────────────────────────────────────────────┘

✅ READY TO BOOK (1 traveler)

┌─────────────────────────────────────────────────────────────────────┐
│ Marc Dubois (Vancouver → USA)                                      │
│ Priority: READY | Status: PROFILE COMPLETE                         │
│                                                                     │
│ ✅ All Requirements Met - Book Immediately                         │
│                                                                     │
│ 💡 OPTIONAL RECOMMENDATION:                                         │
│ • NEXUS Enrollment Suggestion                                      │
│   Benefit: Saves 45+ min at Canada-USA border                     │
│   Cost: $50 for 5 years                                            │
│   Note: Not blocking - can book now and suggest for future trips  │
│                                                                     │
│ ✈️ Ready to Book:                                                  │
│    Alaska Airlines YVR-SEA, aisle seat                            │
│    Hilton Bellevue Diamond (early check-in requested)             │
│    Uber Black ground transport                                    │
│    Estimated: $1,247 CAD                                           │
│                                                                     │
│ [Book Now] [Review Details]                                        │
└─────────────────────────────────────────────────────────────────────┘

SUMMARY:
• 2 travelers waiting on documentation (Priya: visa, Elena: ESTA)
• 1 traveler ready to book immediately (Marc)
• Action Required: Contact Priya and Elena to complete missing requirements
• Estimated Time to Complete: 2-4 weeks (Priya visa), 72 hours (Elena ESTA)
```

---

### Step 5: EA Co-Pilot Books Marc Immediately

```
EA Co-Pilot: "I'm booking Marc now using his preferences and profile data..."

[Know Me provides Marc's complete profile to Booking Agent]

Booking Details:
✈️ FLIGHT: Alaska Airlines AS #521
   YVR → SEA: May 15, 7:45 AM - 9:15 AM
   SEA → YVR: May 17, 6:30 PM - 8:00 PM

   Preferences Applied:
   • Aisle seat (96% confidence)
   • Alaska Airlines (frequent flyer AS123456 - miles credited)
   • No meal preference specified (domestic short flight)

🏨 HOTEL: Hilton Bellevue
   Check-in: May 15 | Check-out: May 17 (2 nights)

   Preferences Applied:
   • Hilton Honors Diamond (12 years)
   • King bed, high floor (94% confidence)
   • Early check-in requested (Marc's preference)
   • Gym access confirmed
   • $220/night (within Marc's typical spend)

🚗 GROUND: Uber Black
   SEA Airport → Hilton Bellevue
   Workshop → SEA Airport (May 17, 3:30 PM)

   Preferences Applied:
   • Uber Black (premium tier, 79% confidence)
   • 30-min buffer before flight (Marc's typical pattern)

📧 CONFIRMATION SENT TO:
   • marc.dubois@sap.com
   • Sarah (EA)

✅ Marc's booking complete! Total: $1,247 CAD
```

---

### Step 6: Know Me Tracks Completion Status

```
EA Co-Pilot Dashboard:

BELLEVUE AI WORKSHOP BOOKING STATUS:

✅ Marc Dubois - BOOKED
   Confirmation: AS521 + Hilton #HB789456
   Status: Complete

⏳ Priya Sharma - WAITING ON VISA
   Blocker: US B1/B2 visa not verified
   Work item created: Apr 21, 2:30 PM
   Assigned to: Sarah (Travel Arranger)
   Expected resolution: 2-4 weeks (visa processing time)
   Status: Pending arranger action

⏳ Elena Müller - WAITING ON ESTA
   Blocker: ESTA authorization not verified
   Work item created: Apr 21, 2:30 PM
   Assigned to: Sarah (Travel Arranger)
   Expected resolution: 72 hours
   Status: Pending arranger action

NEXT ACTIONS FOR SARAH:
• Contact Priya to verify/obtain US visa (high priority)
• Contact Elena to apply for ESTA (high priority)
• Follow up with Priya if no response by Apr 24
• Follow up with Elena if no response by Apr 22
• Book Priya's flight AFTER visa confirmed (closer to travel date)
• Book Elena's flight as soon as ESTA approved
```

---

### Step 7: Profile Updates Received - Know Me Validates

#### **Scenario A: Sarah Contacts Elena, Elena Updates Profile (April 22)**

```
Sarah contacts Elena via email/Slack based on work item

Elena responds:
• ESTA Number: 9876543210 (already had ESTA, applied last year)
• ESTA approved: Apr 15, 2025 (valid until Apr 15, 2027)
• Mobile Phone: +49 173 555 0142

Sarah updates Elena's profile in Know Me

[Know Me Agent validates ESTA]

GET /esta-verification/9876543210
Response: {
  "valid": true,
  "approvalDate": "2025-04-15",
  "expiresDate": "2027-04-15",
  "status": "APPROVED"
}

[Know Me Agent sends SMS verification code to Elena's phone]

SMS to +49 173 555 0142: "Your verification code is 847392"

Sarah provides code to Elena → Elena verifies → Mobile verified ✅

[Know Me Agent notifies EA Co-Pilot]

EA Co-Pilot: "✅ Great news! Elena's profile is now complete.
• ESTA approved (valid until 2027)
• Mobile phone verified: +49 173 555 0142

I'm ready to book her flight. Should I proceed with:
✈️ Lufthansa LH490 MUC-SEA business class (May 14 evening departure)
   Window seat, forward section, gluten-free meal
🏨 Marriott Bellevue - suite (Platinum Elite rate)

Estimated cost: €1,850 total

[Book Now] [Review Options]"
```

#### **Scenario B: Sarah Contacts Priya, Priya Confirms Existing Visa (April 23)**

```
Sarah contacts Priya via email/phone based on work item

Priya provides information:
• US Visa Number: 98765432
• Visa Type: B1/B2 (Business/Tourism)
• Issue Date: 2024-03-15
• Expiration: 2034-03-15
• Email: priya.sharma@sap.com
• Mobile: +91 98765 43210

Sarah updates Priya's profile in Know Me

[Know Me Agent validates visa status]

Validation Result:
✅ US B1/B2 visa valid (10-year visa, expires 2034)
✅ Email verified
✅ Mobile verified (SMS code sent to India number)

⚠️ Passport expiration WARNING remains:
   Expires: Aug 30, 2026 (4 months)
   Recommendation: Renew before travel or risk denial

[Know Me Agent notifies EA Co-Pilot]

EA Co-Pilot: "✅ Priya's visa verified! Profile complete except passport warning.

⚠️ IMPORTANT: Priya's passport expires Aug 30, 2026 (only 4 months from now).
While her visa is valid, USA immigration may deny entry if passport doesn't have
6+ months validity.

RECOMMENDATION FOR SARAH:
Option A: Book flight now + strongly advise Priya to renew passport
Option B: Wait until passport renewed (safer, but delays booking)

I can book:
✈️ Air India AI127 BLR-DEL-SEA (May 14-15, overnight)
   Return: AI128 SEA-DEL-BLR (May 17-18)
   Window seat, vegetarian meal
   Economy class (long-haul 20+ hours)

🏨 Marriott Bellevue - king room (Marriott Gold rate)

Estimated cost: ₹1,85,000 (~$2,200 USD)

Should I:
A) Book now + Sarah warns Priya about passport
B) Wait for passport renewal confirmation

[Book with Warning] [Wait for Passport]"
```

---

### Step 8: Final Booking Summary - All 3 Travelers

```
EA Co-Pilot Final Report:

🎉 ALL TRAVELERS BOOKED FOR BELLEVUE AI WORKSHOP

✅ Marc Dubois (Vancouver)
   Booked: Apr 21 (immediate)
   Flight: Alaska AS521 YVR-SEA
   Hotel: Hilton Bellevue (Diamond, early check-in)
   Cost: $1,247 CAD
   Notes: NEXUS recommendation sent for future trips

✅ Elena Müller (Munich)
   Booked: Apr 22 (after ESTA verification)
   Flight: Lufthansa LH490 MUC-SEA business class
   Hotel: Marriott Bellevue (Platinum suite)
   Cost: €1,850
   Notes: Gluten-free meal pre-ordered, Senator lounge access confirmed

✅ Priya Sharma (Bangalore)
   Booked: Apr 23 (after visa verification)
   Flight: Air India AI127 BLR-DEL-SEA economy
   Hotel: Marriott Bellevue (Gold rate king room)
   Cost: ₹1,85,000 (~$2,200 USD)
   ⚠️ Warning sent: Passport expires soon, renewal recommended

TOTAL TIME SAVED:
• Profile validation: Prevented 2 booking failures (no visa/ESTA)
• Preference application: Saved 45 min booking time (no manual lookups)
• Work item dashboard: Eliminated 6+ back-and-forth email threads with travelers

ISSUES PREVENTED:
• Priya: Would have booked flight → denied boarding (no visa)
• Elena: Would have booked flight → denied boarding (no ESTA)
• Both: Thousands in cancelled flight costs + workshop attendance failure

PREFERENCES HONORED:
• 18 individual preferences automatically applied across 3 travelers
• 95% confidence average (based on historical booking patterns)
• Zero manual preference lookups required
```

---

## Know Me Agent Value Demonstrated

### 1. **Proactive Validation**
- Detected visa/ESTA requirements BEFORE booking
- Prevented $6,000+ in cancelled flights
- Ensured workshop attendance for all employees

### 2. **Personalization at Scale**
- Applied 18 different preferences across 3 travelers
- No manual lookup required - Know Me provided context instantly

### 3. **EA Efficiency**
- Single query returned all validation issues for 3 travelers
- Work item dashboard with actionable tasks for travel arranger
- Dashboard tracking of completion status
- Reduced booking time from 3+ hours → 45 minutes

### 4. **Risk Mitigation**
- Passport expiration warnings
- Contact information validation (SMS/email)
- Border crossing optimization (NEXUS recommendation)
- Compliance with international travel regulations

### 5. **User Experience**
- Travel arranger (Sarah) received clear work items with specific actions needed
- Know Me applied travelers' historical preferences (window seat, vegetarian, business class)
- Seamless mobile verification via SMS (managed by arranger)
- Zero disruption to workshop attendance

---

## Technical Integration Points

### Know Me APIs Used:
1. `GET /know-me/v1/users/{userId}/validation?context=international&destination=US&origin={country}`
2. `POST /know-me/v1/users/{userId}/mobile/verify/send`
3. `POST /know-me/v1/users/{userId}/visa/validate`
4. `GET /know-me/v1/users/{userId}/preferences?context=booking`
5. `GET /esta-verification/{estaNumber}` (external API via Know Me)

### Confidence Scoring Applied:
- Marc's preferences: 88-96% confidence → all auto-applied
- Elena's preferences: 89-100% confidence → all auto-applied
- Priya's preferences: 85-98% confidence → all auto-applied


## Outcome

✅ **3 international travelers successfully booked**
✅ **2 booking failures prevented** (visa/ESTA)
✅ **18 preferences honored** (95% confidence avg)
✅ **$6,000+ in cancellation fees avoided**
✅ **3+ hours EA time saved**
✅ **100% workshop attendance achieved**
