# Profile Know Me Agent – Requirements Document

## 1. Overview

The Profile Agent ("Know Me") is a platform-level service that maintains a comprehensive, context-aware user profile enabling personalized experiences across all SAP Concur travel and expense products.

**Key goal: Universal user intelligence.**

"Know Me" serves as the single source of truth for user context and intelligence. When any agent in Concur needs to understand a user—whether for booking recommendations, expense validation, policy guidance, or proactive assistance—"Know Me" provides:

### Personal & identity information
- Contact details (email, phone number, address)
- Travel documents (passport information, visa details)
- Work information (cost center, department, manager, legal entity)
- Account details (employee ID, user role, permissions)

### Preferences & behavioral intelligence
- User preferences (red-eye flights, Marriott loyalty, window seats)
- Behavioral patterns (parks at PAE, flies from SEA, books 2 weeks ahead)
- Relationships (delegates, approvers, frequent travelers with)
- Loyalty programs (Marriott Gold, United Premier)
- Context (role, travel frequency, typical destinations, business justifications)

### Profile Validation & Completeness
- Detects missing required fields for bookings (passport for international, loyalty numbers, TSA PreCheck)
- Proactively prompts conversationally to complete profile during booking flow
- Prevents booking failures by validating profile completeness before transaction
- Updates the profile in real time as users provide information during conversations

---

## 2. Core Objectives

- Learn user preferences automatically from behavior without requiring manual configuration
- Detect missing profile fields required for specific travel scenarios and prompt users conversationally
- The Know Me agent centralizes all user profile data within a single platform service, allowing other agents to retrieve only relevant profile fields based on their use cases
- Know Me agent helps predict user behavior and patterns that enable Travel and Expense agents to make intelligent decisions and recommendations and predictions
- Maintain data quality, privacy controls, and consent management

---

## 3. Functional Requirements

### 3.1 Identity & Core Profile Management

**FR-1.1: Store and manage core identity attributes:**
- Legal name, preferred name, nicknames
- Home address, work address, emergency contacts
- Contact preferences (email, SMS, iMessage, phone)
- Language and timezone preferences
- Admin/delegate assignments (e.g., Angela Demele for Fred)

**FR-1.2: Maintain organizational context:**
- Employer (SAP), department, role
- Manager hierarchy and approval chain
- Cost center and budget allocation
- Travel policy tier and spending limits

**FR-1.3: Support profile data writes via user JWT (per CP-35908):**
- Allow users to update their own profile fields
- Scope-based field-level access control (sensitive vs. non-sensitive)
- Audit logging for all profile changes

### 3.2 Loyalty & Membership Programs

**FR-2.1: Store loyalty program memberships:**
- Program name (Marriott Bonvoy, United MileagePlus)
- Membership number, tier/status (Platinum Elite, 22-year member)
- Expiration dates, points balance (if available via API)
- Program-specific preferences (e.g., Marriott = always first choice)

**FR-2.2: Track tier benefits and entitlements:**
- Elite hold inventory access
- Upgrade eligibility (Lyft Extra Comfort)
- Lounge access, priority boarding

**FR-2.3: Store travel program enrollments:**
- TSA PreCheck, Global Entry, Clear
- Known Traveler Number (KTN)
- NEXUS, SENTRI (border crossing programs)

### 3.3 Travel Preferences (Learned & Explicit)

**FR-3.1: Flight preferences:**
- Seat class (First class)
- Seat position (Window, 4A preferred)
- Time of day (red-eye, morning, afternoon)
- Airlines (preferred/avoided based on loyalty or experience)
- Direct vs. connections tolerance

**FR-3.2: Hotel preferences:**
- Brand loyalty (Marriott always first choice)
- Specific properties (Marriott Union Square)
- Room type (high floor, king bed, suite)
- Amenities (gym, pool, business center)

**FR-3.3: Ground transportation:**
- Preferred service (Lyft over Uber)
- Service tier (Extra Comfort upgrade)
- Pickup locations (airport vs. hotel)

**FR-3.4: Dining preferences:**
- Cuisine types (Greek, Steakhouse, American, Vietnamese, Seafood)
- Dietary restrictions (allergies, vegetarian, kosher, halal)
- Preferred dining time (7:00 PM for Fred)
- Party size context (solo traveler vs. business dinner)

### 3.4 Personal Relationships & Context

**FR-4.1: Maintain personal relationships:**
- Spouse/partner (Lisa Fredericks)
- Communication channel (iMessage preferred)
- Known gesture patterns (send flowers when missing plans)

**FR-4.2: Store behavioral patterns:**
- Gift preferences (flowers for Lisa)
- Apology patterns (message templates)
- Anniversary/birthday reminders

**FR-4.3: Track trip companions:**
- Frequent travel companions (colleagues, clients)
- Co-traveler preferences (shared rooms, separate cars)

---

## Example Response

### Agent Query: "Tell me about Fred Fredericks for booking"

**Know Me Response (scoped to booking use case):**

#### Personal Information:
- Name: Fred Fredericks
- Email: fred.fredericks@sap.com
- Phone: +1 (703) 555-0142
- Home Address: Fred's House, Alexandria, VA
- Employer: SAP
- Cost Center: ENG-SALES-VA
- Admin/Delegate: Angela Demele

#### Travel Intelligence:
- First Class preference, window seat 4A (97% confidence, 15 trips)
- United MileagePlus Premier Silver member
- Marriott Bonvoy Platinum Elite (22-year member, always first choice)
- Preferred hotel: Marriott Union Square, high floor
- Books 12-15 days in advance
- Travels 2-3 times per month (San Francisco, NYC, Chicago)
- Ground transport: Lyft Extra Comfort (preferred upgrade tier)
- TSA PreCheck enrolled: KTN 12345678901

#### Loyalty Programs:
- Marriott Bonvoy: MB123456789 (Platinum Elite, elite-hold inventory access)
- United MileagePlus: UA987654321 (Premier Silver)
- TSA PreCheck: 12345678901

#### Behavioral Patterns:
- Solo traveler (party of 1)
- Preferred dinner time: 7:00 PM
- Cuisine preferences: Greek, Steakhouse, American, Vietnamese, Seafood
- Typical meal budget: $45-65 (dinner)

#### Predictive Intelligence:
- Next likely trip: San Francisco (quarterly pattern, due for visit)
- Expected booking: United First Class 4A, Marriott Union Square high floor
- Probable dinner reservation: 7:00 PM, party of 1

#### Profile Validation:
✓ All required fields present for domestic booking
✓ TSA PreCheck enrolled
✓ Loyalty programs active
✓ International travel ready
⚠ Recommendation: Add Global Entry for enhanced international travel efficiency

#### Contextual Notes:
- Spouse: Lisa Fredericks (preferred contact: iMessage)
- Known pattern: Sends apology flowers when missing personal plans for business travel
- 100% policy compliance history
- Premium travel preferences aligned with SAP travel policy

---

## 4. Feature 1: Missing Profile Field Detection & Conversational Update

### 4.1 Scenario-Based Field Validation

**FR-5.1: Validate profile completeness against travel scenario requirements:**

Trip context examples:
- International flight to UK: Requires passport number, passport expiration, middle name
- TSA PreCheck eligible flight: Requires Known Traveler Number (KTN)
- Marriott booking with loyalty: Requires Marriott Bonvoy number
- Lyft reservation: Requires mobile phone number
- Business dinner at OpenTable: Requires dietary preferences, party size

**FR-5.2: Detect missing fields before booking:**
- Query: "What fields does Fred need for [international flight to London]?"
- Response: Missing passport number; passport expires in 3 months (warn); middle name required by airline

**FR-5.3: Prioritize missing fields by impact:**
- **Blocking**: Passport for international (cannot book without)
- **Warning**: Passport expiring in 4 months (may cause issues)
- **Recommended**: Frequent flyer number (won't block, but user loses miles)

### 4.2 Conversational Profile Update Interface

**FR-5.4: Prompt user conversationally when missing fields are detected.**

Example flow:

```
Joule: "Fred, I'm booking your flight to London. I need a few details first:
- Passport number (required for international travel)
- Passport expiration date (must be valid 6+ months beyond your trip)
- United MileagePlus number (optional — so you earn miles)

Can I collect these now, or would you like to skip the MileagePlus for now?"

Fred: "My passport is X12345678, expires June 2028."

Joule: "Got it! Passport saved. What about your MileagePlus number?"

Fred: "Skip that for now."

Joule: "No problem. You can add it later in your profile if you'd like to earn miles on future flights."
```

**FR-5.5: Support inline profile updates without leaving the booking flow:**
- User stays in Joule chat or booking interface
- Fields updated via conversational input (text or voice)
- Profile Agent writes directly to Identity Service via user JWT
- Confirmation shown: "Passport saved to your profile"

**FR-5.6: Provide context for why each field is needed:**
- "Passport required for UK entry"
- "Middle name required by United for international flights"
- "TSA PreCheck speeds up security (you'll save ~15 minutes)"

**FR-5.7: Allow partial completion and defer non-blocking fields:**
- User can skip optional fields (e.g., frequent flyer)
- Remind later: "Add MileagePlus number before your next United flight?"
- Track completion rate: "Your profile is 85% complete"

### 4.3 Proactive Profile Maintenance

**FR-5.8: Alert users to expiring credentials:**
- "Fred, your passport expires in 6 months. Renew before your September trip to Tokyo."
- "Your TSA PreCheck expires next month. Renew now to keep expedited security."

**FR-5.9: Suggest profile improvements based on upcoming travel:**
- "You have 3 Marriott stays coming up. Add your Bonvoy number to earn 45,000 points."
- "You're flying United 5 times this quarter. Add your MileagePlus number to track status."

**FR-5.10: Post-trip profile enrichment:**
- After trip: "I noticed you upgraded to Lyft Extra Comfort. Should I make that your default?"
- After hotel stay: "You stayed at Marriott Union Square for the 16th time. Should I always prioritize this property in SF?"

---

## 5. Feature 2: Dynamic Learning & Behavioral Pattern Recognition

### 5.1 Automatic Preference Learning

**FR-6.1: Learn preferences from booking behavior without manual input:**

Learning sources:
- Past bookings (flights, hotels, cars, restaurants)
- Expense submissions (receipts, merchant patterns)
- Calendar events (meeting patterns, travel frequency)
- Geo-location patterns (home airport, frequent destinations)

**FR-6.2: Detect patterns across multiple dimensions:**

**Flight patterns:**
- Time of day (80% of Fred's flights are after 6pm → prefers evening departures)
- Seat selection (100% window seats → strong preference)
- Airlines (90% United → loyalty-driven)
- Cabin class (95% first class → willing to pay for comfort)

**Hotel patterns:**
- Brand loyalty (22 years Marriott, 85% of stays → always prioritize Marriott)
- Room floor (12 bookings requested high floor → consistent preference)
- Property preference (15 stays at Marriott Union Square → favorite property in SF)

**Ground transport patterns:**
- Service provider (70% Lyft vs 30% Uber → prefers Lyft)
- Service tier (60% Extra Comfort upgrades → willing to pay for space)
- Pickup timing (books rides 30 min before flight → buffer preference)

**Dining patterns:**
- Cuisine variety (Greek, Steakhouse, American, Vietnamese, Seafood → adventurous eater)
- Solo vs. group (80% solo dinners at 7pm → prefers dinner alone after work)
- Restaurant type (mid-to-high-end, no fast food → quality preference)

**Expense patterns:**
- Parking location (always parks at PAE, flies from SEA → knows this saves money)
- Meal costs (average $45 dinner when solo, $120 when with clients → context-aware spending)
- Receipt submission timing (submits within 2 days of trip → organized traveler)

### 5.2 Confidence Scoring & Pattern Validation

**FR-6.3: Calculate confidence scores for each learned preference:**

**Confidence formula:**
```
Confidence = (Frequency × 40%) + (Consistency × 40%) + (Recency × 20%)
```

**Example: Fred's window seat preference**
- Frequency: 25 bookings / 25 total = 100% (40 points)
- Consistency: 25 out of 25 = 100% (40 points)
- Recency: Last booking 2 weeks ago = 90% (18 points)
- **Total Confidence: 98%**

**FR-6.4: Expose confidence levels to users and agents:**
- High confidence (>90%): "You always book window seats" → auto-apply
- Medium confidence (70–90%): "You usually prefer Marriott" → suggest but don't auto-apply
- Low confidence (<70%): "You sometimes book Lyft" → neutral, don't recommend

**FR-6.5: Track pattern changes and adapt:**
- Detect shift: Fred books 3 aisle seats in a row after 25 window seats
- Lower confidence: 98% → 75%
- Prompt for confirmation: "I noticed you've been booking aisle seats. Should I update your preference?"

### 5.3 Preference Recommendations to User

**FR-6.6: Proactively suggest preferences based on learned patterns:**

**After 5 trips:**
```
Joule: "Fred, I've noticed you always book window seats. Should I make 'window seat'
your default preference? This will save you time on future bookings."

Options:
• Yes, always prefer window
• Ask me each time
• No preference
```

**After 10 Marriott stays:**
```
Joule: "You've stayed at Marriott for your last 10 business trips.
Should I always show Marriott hotels first when you search?"

Confidence: 95% (10 data points, 100% consistency)

Options:
• Yes, prioritize Marriott
• Show all options equally
• No, I want variety
```

**After expense submission pattern:**
```
Joule: "I noticed you always park at Paine Field (PAE) when
flying from SeaTac (SEA). This is unusual but makes sense
since PAE is closer to your home.

Should I remember this pattern to validate future parking expenses?"

Context: 6 past trips, saves $10/day vs. SEA parking

Options:
• Yes, this is my normal pattern
• Ask me each time
```

**FR-6.7: Allow users to accept, reject, or modify recommendations:**
- Accept → Store as explicit preference (confidence = 100%)
- Reject → Flag as "user explicitly declined" (stop suggesting)
- Modify → User provides alternate preference (e.g., "aisle on short flights, window on long flights")

**FR-6.8: Batch recommendations in "Profile Review" sessions:**
- Monthly digest: "Review 5 new preferences I learned about you"
- Reduces notification fatigue
- User reviews all at once, approves/rejects in bulk

### 5.4 Predictive Expense & Travel Behavior

**FR-6.9: Predict expenses before they occur based on historical patterns:**

**Expense predictions:**

**Hotel cost prediction:**
```
Context: Fred booking 3-night SF trip
Historical data: 15 past SF trips, avg $450/night at Marriott Union Square
Confidence: 92%
```

**Meal expense prediction:**
```
Context: Solo business trip, 3 days
Historical pattern: Fred spends avg $45/dinner when solo
Prediction: "$135 in meal expenses (3 dinners × $45 avg)"
Confidence: 85%
Note: Client dinners average $120, budget accordingly if hosting
```

**Ground transport prediction:**
```
Context: Airport transfer
Historical pattern: Fred books Lyft Extra Comfort ($65 avg)
Prediction: "$130 round-trip airport transfers"
Confidence: 78%
```

**FR-6.10: Pre-populate expense reports with predicted amounts:**
- After trip: "I've drafted your expense report based on your typical spending"
- Pre-filled amounts: Hotel $1,350, Meals $135, Lyft $130
- User confirms or adjusts actual amounts
- Reduces expense submission time by 80%

**FR-6.11: Flag anomalies in real time:**
- Fred submits $200 parking at PAE (usual: $45 for 3 days)
- Alert: "This is 4× your typical PAE parking cost. Is this correct?"
- Context: Helps catch errors before submission

**FR-6.12: Trip cost predictions before booking:**
```
Joule: "Your SF trip next week will cost approximately:
- Airfare: $650 (first class, typical for you)
- Hotel: $1,350 (3 nights Marriott Union Square)
- Ground: $130 (Lyft transfers)
- Meals: $135 (solo dinners)
Total estimate: $2,265

This is within your monthly travel budget of $8,000."
```

### 5.5 Behavioral Pattern Personalization

**FR-6.13: Use learned patterns to personalize every interaction:**

**Booking personalization:**
- Show Marriott Union Square first in SF hotel results (property-level preference)
- Filter flights to show evening departures first (time preference)
- Pre-select window seat 4A when available (seat preference)
- Auto-suggest Lyft Extra Comfort for airport pickup (ground transport preference)

**Expense personalization:**
- Auto-categorize Marriott charges as "Lodging" (learned from past)
- Pre-approve PAE parking for SEA flights (geographic pattern)
- Flag $120 solo dinners as unusual (typical solo: $45)
- Suggest split-billing for client dinners (learned business rule)

**Communication personalization:**
- Send Lisa iMessage when Fred's flight is delayed (relationship + channel preference)
- Proactively offer to send flowers when Fred extends trip (gesture pattern)
- Use conversational tone in Joule (Fred prefers casual AI interactions)

**FR-6.14: Contextual recommendations based on trip purpose:**

**Business Trip to SF (with client meeting):**
- Hotel: Marriott Union Square (proximity to client office)
- Dinner: Steakhouse reservation at 7pm for 2 (client entertainment pattern)
- Budget: $120 meal estimate (client dinner average)

**Personal Trip to SF (solo):**
- Hotel: Marriott Union Square (same preference, personal rate)
- Dinner: Vietnamese or Greek at 7pm for 1 (solo adventure pattern)
- Budget: $45 meal estimate (solo dinner average)

**FR-6.15: Cross-domain learning (expense informs booking, booking informs expense):**

**Example 1: Parking Pattern → Booking Recommendation**
- Learned: Fred parks at PAE (6 past trips)
- Booking context: Fred searches SEA flights
- Recommendation: "Book 6am flight from SEA to give time to drive from PAE? Or search PAE flights?"

**Example 2: Meal Pattern → Calendar Integration**
- Learned: Fred eats solo at 7pm when no evening meetings
- Booking context: Trip to SF, calendar shows 6pm meeting Wed
- Recommendation: "Dinner reservation at 8pm Wednesday (you have a 6pm meeting)?"

**Example 3: Hotel Loyalty → Expense Validation**
- Learned: Fred is Marriott Platinum (entitled to upgrades)
- Expense context: $500/night charge (above policy $300 limit)
- Smart validation: "Upgraded room due to Platinum status (policy exception approved)"

---

## 6. Non-Functional Requirements

### 6.1 Privacy & Security
- **NFR-1.1**: Field-level sensitivity classification (Public, Internal, Sensitive, Restricted)
- **NFR-1.2**: Scope-based access control (profile:basic:read, profile:travel:read, profile:sensitive:read)
- **NFR-1.3**: Purpose-based access logging (X-Request-Purpose header required)
- **NFR-1.4**: Data masking for sensitive fields (passport: X1234****, SSN: ***-**-5678)
- **NFR-1.5**: GDPR compliance: user data export, deletion, access history dashboard
- **NFR-1.6**: Learning opt-out: Users can disable behavioral tracking

### 6.2 Learning Accuracy
- **NFR-2.1**: Pattern detection accuracy: >85% for preferences with 5+ data points
- **NFR-2.2**: Expense prediction accuracy: ±15% of actual spend
- **NFR-2.3**: False positive rate: <10% (avoid incorrect pattern detection)
- **NFR-2.4**: Adaptation speed: Detect pattern shifts within 3 data points

---

## 7. User Controls

- **UC-1**: Profile visibility dashboard (what agents see about me)
- **UC-2**: Preference override ("Always ask, never auto-book")
- **UC-3**: Learning transparency ("Show me what patterns you've learned")
- **UC-4**: Preference correction ("This pattern is wrong, delete it")
- **UC-5**: Data deletion (right to be forgotten)
- **UC-6**: Learning opt-out (disable automatic preference detection)
- **UC-7**: Prediction visibility ("Why did you predict $1,350 hotel cost?")
- **UC-8**: Audit log (who accessed my profile and why)

---

## 8. Success Metrics

- **M-1**: Profile completeness: % of users with 80%+ fields populated
- **M-2**: Conversational update success rate: % of users who complete profile via Joule prompts
- **M-3**: Preference accuracy: % of AI recommendations accepted by users
- **M-4**: Booking time reduction: average time from search to confirmation (target: -50%)
- **M-5**: Expense prediction accuracy: % within ±15% of actual spend
- **M-6**: Auto-approval rate increase: % of expenses auto-approved due to pattern recognition
- **M-7**: Learning adoption: % of users with 5+ learned preferences
- **M-8**: Anomaly detection rate: % of expense errors caught before submission

---

## 9. Open Questions

- **Passive vs. active learning**: Should we prompt users to confirm every learned preference, or only high-impact ones?
- **Pattern expiration**: How long should we retain old patterns that haven't been reinforced (6 months? 1 year)?
- **Multi-profile support**: How do we handle users with separate work/personal travel profiles?
- **Prediction transparency**: How much detail should we show about why predictions were made?
