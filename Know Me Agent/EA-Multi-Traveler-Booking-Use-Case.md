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

### Step 4: EA Takes Action - Profile Completion

```
EA (Sarah): "Send profile completion requests to Priya and Elena. Book Marc now
using his preferences."

EA Co-Pilot: "Perfect! I'm sending completion requests now..."

[Know Me Agent generates personalized work items]
```

#### **Email to Priya Sharma:**

```
Subject: 🚨 Action Required: Travel Documents for Bellevue Trip (May 15-17)

Hi Priya,

Sarah is booking your travel to Bellevue, WA for the AI workshop (May 15-17),
but I've identified critical issues that will prevent your booking:

🚨 BLOCKING ISSUES:
• US Visa: I don't have a valid US visa on file for you. As an Indian citizen,
  you need a B1/B2 visa to enter the USA.

  ACTION:
  - If you already have a valid US visa, please add it to your profile here:
    [Update Visa Information]
  - If you need to apply, start immediately (processing takes 2-4 weeks):
    https://ustraveldocs.com/in/

• Passport Expiration: Your passport expires Aug 30, 2026 (only 4 months from now).
  USA requires 6+ months validity. You may be denied boarding or entry.

  ACTION: Begin passport renewal process today.

📧 CRITICAL - Missing Contact Information:
• Email: I need your email address to send your boarding pass and flight updates
• Mobile: I need your mobile number for SMS gate change alerts (critical for
  your BLR-SEA connection)

⏰ URGENT: Without a valid visa, I cannot book your flight. Please update your
profile within 3 days to allow enough time for visa processing.

[Update My Profile] [I Have Questions]

Based on your past trips, I'll book:
✈️ Window seat, vegetarian meal
🏨 Marriott Bonvoy Gold - king room
📍 Bellevue Marriott (close to workshop venue)

Best,
Joule (EA Co-Pilot for Sarah)
```

#### **Email to Elena Müller:**

```
Subject: ⚠️ Action Required: ESTA Needed for USA Trip (May 15-17)

Hi Elena,

Sarah is booking your travel to Bellevue for the AI workshop (May 15-17).
Good news: Your profile is mostly complete! But I need one critical item:

⚠️ ESTA Authorization Required:
As a German citizen traveling to the USA, you need ESTA (Electronic System
for Travel Authorization) approval before boarding your flight.

ACTION NEEDED:
• Apply now at: https://esta.cbp.dhs.gov
• Cost: $21 USD
• Processing: Up to 72 hours (apply TODAY to ensure approval before flight booking)
• Once approved, add your ESTA number here: [Update ESTA]

📱 ALSO MISSING: Mobile Phone Number
• I need your mobile number to send SMS alerts for gate changes and delays
• Critical for international flights with connections
• Add here: [Update Mobile Number]

⏰ TIMELINE:
I'll wait for your ESTA approval before booking your flight. Please apply
within 24 hours to stay on schedule.

Based on your preferences, I'll book:
✈️ Lufthansa MUC-SEA, window seat forward section, gluten-free meal
✈️ Business class (long-haul, 10+ hours - your typical preference)
🏨 Marriott Bonvoy Platinum - suite or high floor king
🚗 Premium ground transport
📍 Bellevue Marriott (Senator lounge access at SEA airport on return)

[Complete My Profile] [I Have Questions]

Best,
Joule (EA Co-Pilot for Sarah)
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
   Profile update request sent: Apr 21, 2:30 PM
   Expected resolution: 2-4 weeks (visa processing time)
   Status: Pending traveler action

⏳ Elena Müller - WAITING ON ESTA
   Blocker: ESTA authorization not verified
   Profile update request sent: Apr 21, 2:30 PM
   Expected resolution: 72 hours
   Status: Pending traveler action

NEXT ACTIONS:
• Follow up with Priya if no response by Apr 24
• Follow up with Elena if no response by Apr 22
• Consider booking Priya's flight AFTER visa confirmed (closer to travel date)
• Book Elena's flight as soon as ESTA approved
```

---

### Step 7: Profile Updates Received - Know Me Validates

#### **Scenario A: Elena Updates Profile (April 22)**

```
Elena updates profile:
• ESTA Number: 9876543210 (approved Apr 21)
• Mobile Phone: +49 173 555 0142

[Know Me Agent validates ESTA]

GET /esta-verification/9876543210
Response: {
  "valid": true,
  "approvalDate": "2026-04-21",
  "expiresDate": "2028-04-21",
  "status": "APPROVED"
}

[Know Me Agent sends SMS verification code to Elena's phone]

SMS to +49 173 555 0142: "Your verification code is 847392"

Elena enters code → Mobile verified ✅

[Know Me Agent notifies EA Co-Pilot]

EA Co-Pilot: "✅ Great news! Elena's profile is now complete.
• ESTA approved (valid until 2028)
• Mobile phone verified: +49 173 555 0142

I'm ready to book her flight. Should I proceed with:
✈️ Lufthansa LH490 MUC-SEA business class (May 14 evening departure)
   Window seat, forward section, gluten-free meal
🏨 Marriott Bellevue - suite (Platinum Elite rate)

Estimated cost: €1,850 total

[Book Now] [Review Options]"
```

#### **Scenario B: Priya Confirms Existing Visa (April 23)**

```
Priya updates profile:
• US Visa Number: 98765432
• Visa Type: B1/B2 (Business/Tourism)
• Issue Date: 2024-03-15
• Expiration: 2034-03-15
• Email: priya.sharma@sap.com
• Mobile: +91 98765 43210

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

RECOMMENDATION:
Option A: Book flight now + strongly advise passport renewal
Option B: Wait until passport renewed (safer, but delays booking)

I can book:
✈️ Air India AI127 BLR-DEL-SEA (May 14-15, overnight)
   Return: AI128 SEA-DEL-BLR (May 17-18)
   Window seat, vegetarian meal
   Economy class (long-haul 20+ hours)

🏨 Marriott Bellevue - king room (Marriott Gold rate)

Estimated cost: ₹1,85,000 (~$2,200 USD)

Should I:
A) Book now + warn Priya about passport
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
• Automated follow-up: Eliminated 6+ back-and-forth emails

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
- Automated follow-up emails with personalized requirements
- Dashboard tracking of completion status
- Reduced booking time from 3+ hours → 45 minutes

### 4. **Risk Mitigation**
- Passport expiration warnings
- Contact information validation (SMS/email)
- Border crossing optimization (NEXUS recommendation)
- Compliance with international travel regulations

### 5. **User Experience**
- Travelers received personalized emails explaining exactly what they needed
- Know Me applied their historical preferences (window seat, vegetarian, business class)
- Seamless mobile verification via SMS
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
