# Travel Preferences and Behavioral Intelligence

This document contains field categories for travel preferences and behavioral learning in the Know Me Agent. These fields enable personalized booking experiences by learning from user behavior and storing explicit preferences.

---

# **Static Preferences**

Static preferences are preferences that currently exist in the Concur UI and system. These fields are explicitly set by users or administrators in their travel profile and are stored in the Travel Profile Service (TPS). Static preferences provide the baseline for personalized booking experiences and are supplemented by behavioral intelligence that Know Me Agent learns over time.

---

## **Travel Preferences** (Pre-fill Booking Requests)

These fields automatically apply learned or explicitly stated preferences during booking, saving time and ensuring comfort.

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **Home Airport** | Text (IATA) | 3 chars | Pre-fills departure airport in search, saving time. AI agents recommend flights from preferred airport automatically. | **Saves 30+ seconds per search** - Customer doesn't manually type airport every time. Prevents accidental bookings from wrong airport (e.g., searches SFO but lives near OAK). Reduces booking errors and frustration. |
| **Air Seat Position (Row)** | Choice | — | Auto-selects aisle/window seat during booking. Ensures comfort preference honored without manual seat map navigation. | **Prevents uncomfortable flights** - Customer forgets to select seat, gets stuck in middle seat on 6-hour flight (extremely uncomfortable). Aisle preference for frequent bathroom needs, window for sleeping. Auto-selection ensures preferred experience every time. |
| **Air Seat Position (Section)** | Choice | — | Selects preferred cabin location (exit row for legroom, forward for quick deplaning, rear for quieter). | **Comfort and convenience** - Customer stuck in back row (loud engines, slow deplaning—extra 20 min wait). Exit row gives 4+ inches extra legroom critical for tall travelers. Forward section deplanes first, saving time at destination. |
| **Air Meal Preference** | Choice | — | Pre-orders special meals (vegetarian, kosher, gluten-free, diabetic). Airlines require 24-72 hours advance notice. | **No suitable meal on flight** - Customer with dietary restriction (vegetarian, allergic to gluten) has no meal option on international flight (10+ hours). Must go hungry or risk allergic reaction. Last-minute special meal requests often declined—too late to fulfill. |
| **Hotel Room Type** | Choice | — | Pre-selects king bed, double queen, accessibility room. Ensures room type availability at time of booking. | **Wrong room type at check-in** - Customer arrives at hotel requesting king bed but only double queens available (uncomfortable for couples). Accessibility rooms unavailable for disabled travelers. Must accept unsuitable room or find different hotel (time lost, higher prices). |
| **Hotel Smoking Preference** | Choice | — | Ensures non-smoking room assignment. Smoking rooms smell strongly of cigarette smoke even when unoccupied. | **Health and comfort issue** - Non-smoking customer assigned smoking room, cannot sleep due to smell/allergies. Hotel fully booked, cannot switch rooms. Ruins entire stay. Must pay for room despite unusable conditions. |
| **Car Type Preference** | Choice | — | Pre-selects car size (compact for city, SUV for family/luggage). Ensures availability and avoids mismatches. | **Wrong vehicle for needs** - Customer books compact car, arrives with 4 colleagues + luggage (doesn't fit). Must upgrade on-site at 2-3× price or make multiple trips. SUV needed for winter mountain roads but compact provided—unsafe driving conditions. |
| **Car Transmission** | Choice | — | Critical for international travel where manual transmission is standard. US drivers unfamiliar with manual cannot drive safely. | **Cannot drive rental car** - Customer cannot drive manual transmission, rental counter has no automatics available (common in Europe). Stranded at airport, forced to pay premium for automatic or cancel plans. Safety risk if attempts manual without experience. |

---

## **Rate Preferences** (Discount Eligibility)

These flags indicate eligibility for special discount rates, enabling automatic application of savings at hotels and car rentals.

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **AAA Rate** | Yes/No | — | Discount rate (10-15%) for AAA members at hotels and car rentals. Requires showing AAA card at check-in. | **Modest savings** - Saves $10-30 per hotel night. Customer must remember to request AAA rate and show card at check-in. If missing from profile, customer can request rate manually during booking. Helpful savings but not mandatory. |
| **AARP Rate** | Yes/No | — | Senior discount (10-20%) for AARP members (50+ years old). Common at hotels and car rentals. | **Senior savings** - Saves $15-40 per night for eligible travelers. Must show AARP card at check-in. If missing, customer requests rate manually. Nice discount but not required for booking. |
| **Government Rate** | Yes/No | — | Special government employee rates at hotels (up to 30% discount). Requires showing government ID at check-in. | **Significant savings** - Government rates can be 30%+ cheaper. Hotels set aside inventory for government travelers. Must verify eligibility at check-in (government ID). If missing, customer can request manually but inventory may be sold out. |
| **Military Rate** | Yes/No | — | Military/veteran discount at hotels, car rentals, airlines (10-25% savings). Requires military ID verification. | **Honor and savings** - Rewards military service with meaningful discounts. Must show military/veteran ID at check-in. If missing from profile, customer can request rate manually. Helpful but not blocking. |

---

## **Rail-Specific Preferences**

Preferences specific to train travel, particularly relevant for European rail bookings and Deutsche Bahn.

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Rail Seat** | Choice | — | Aisle vs. window preference for rail travel. Less critical than air travel since trains have more movement freedom. | **Minor comfort** - Nice to have preferred seat but customer can move around on train (unlike plane). If missing, customer selects during booking or accepts assigned seat. Lower impact than flight seating. |
| **Rail Coach Type** | Choice | — | Quiet car, table seating, compartment, or standard coach. Affects comfort for long journeys. | **Comfort preference** - Quiet car nice for work/sleep on long trips. Table seating good for groups. If missing, customer gets standard coach (acceptable for most). Can upgrade on board if available. |
| **Rail Noise Comfort** | Choice | — | Quiet zone vs. mobile phone allowed zones. European trains often have designated quiet carriages. | **Comfort preference** - Quiet zone provides peaceful work environment. Mobile zone allows calls. If missing, customer assigned to standard car (moderate noise). Can move between cars if needed. |
| **BahnCard Type** | Choice | — | Deutsche Bahn discount card (25%, 50%, or 100% off). Significant savings for frequent German rail travelers. | **Major savings for frequent travelers** - BahnCard 50 saves 50% on every German rail ticket. BahnCard 100 = unlimited travel. If missing, customer pays full price (can manually enter discount card at booking). Critical for frequent Germany travelers but niche globally. |
| **BahnCard Class** | Choice | — | First class vs. second class BahnCard. Determines which class receives discount. | **Discount application** - Ensures discount applied to correct class. If missing, discount may apply to wrong class (customer pays more or gets downgraded). Can correct at booking but requires attention. |

---

## **Hotel Amenities**

Preferences for hotel features and accommodations that enhance comfort and convenience during stays.

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Prefer Foam Pillows** | Yes/No | — | Special pillow request for sleep comfort or allergies. Hotels provide on request at check-in. | **Minor comfort** - Improves sleep quality for some guests (allergies, neck support). If missing, customer requests at check-in (usually available). Not guaranteed—depends on hotel inventory. Low impact for most travelers. |
| **Prefer Crib** | Yes/No | — | Baby crib for families traveling with infants. Standard hotel accommodation for families. | **Family convenience** - Hotels provide cribs for free on request. If missing from profile, parent requests at check-in (always available unless hotel fully booked). Minor convenience—parents remember to ask anyway. |
| **Prefer Rollaway Bed** | Yes/No | — | Extra bed for families or groups. Subject to room size and hotel availability. | **Extra guest accommodation** - Allows extra person in room (saves booking second room). If missing, customer requests at check-in ($20-50 fee). May not be available if hotel busy. Helpful but customer can handle ad-hoc. |
| **Prefer Gym** | Yes/No | — | Filters hotel search to properties with fitness centers. Important for fitness-conscious travelers. | **Fitness routine maintenance** - Helps travelers maintain workout routine during travel. If missing, customer can filter manually during search or use nearby gym. Convenience preference but not critical. |
| **Prefer Pool** | Yes/No | — | Filters hotels with swimming pools. Relevant for leisure travelers, families, or fitness swimmers. | **Leisure/fitness preference** - Nice for relaxation or exercise. If missing, customer can filter manually during booking. More relevant for leisure travel than business. Low impact for most business travelers. |
| **Prefer Restaurant** | Yes/No | — | On-site restaurant for convenience (breakfast, room service). Helpful when arriving late or in remote areas. | **Convenience** - Saves time finding nearby food (especially late arrivals). If missing, customer can filter manually or use delivery/nearby restaurants. Helpful but workarounds exist easily. |
| **Prefer Room Service** | Yes/No | — | In-room dining for convenience or privacy. Common for business travelers working late. | **Convenience** - Enables working through dinner in room. If missing, customer orders from restaurant or delivery. Nice-to-have but not necessary—alternatives readily available. |
| **Prefer Early Check-In** | Yes/No | — | Request early check-in (before standard 3 PM). Useful for early morning arrivals from overnight flights. | **Convenience after long flight** - Allows immediate rest after overnight international flight. If missing, customer requests at check-in (subject to availability, often $50 fee). Can wait in lobby if room not ready. |
| **Prefer Wheelchair Access** | Yes/No | — | ADA-compliant room with wheelchair accessibility. Critical for disabled travelers but covered under legal requirements. | **Accessibility requirement** - Hotels legally required to provide accessible rooms under ADA. If missing from profile, customer requests at booking or check-in (usually available). Should be in "Recommended" for affected travelers but optional for general population. |
| **Prefer Access for Blind** | Yes/No | — | Rooms with visual accessibility features (Braille signage, audio alerts, etc.). | **Accessibility requirement** - Hotels provide accessible features on request. If missing, customer requests at check-in. Limited inventory—advance notice helpful. Should be "Recommended" for visually impaired travelers. |

---

## **Car Rental Add-Ons**

Optional equipment and features for rental vehicles.

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **GPS** | Yes/No | — | Rental car GPS navigation system. Less relevant in smartphone era (Google Maps, Waze). | **Legacy convenience** - GPS units useful before smartphones. Now most travelers use phone navigation (free, better, real-time traffic). Rental GPS adds $10-15/day (expensive). If missing, customer uses phone—better solution anyway. |
| **Ski Rack** | Yes/No | — | Roof-mounted ski rack for winter sports trips. Niche use case for specific destinations. | **Niche equipment** - Only relevant for ski trips. If missing, customer rents rack at car counter ($20-30) or checks skis at airline. Very specific use case—not relevant for 95%+ of rentals. |
| **Car Smoking Preference** | Yes/No | — | Smoking vs. non-smoking rental car. Most rental fleets now entirely non-smoking due to health regulations. | **Obsolete preference** - Nearly all rental cars non-smoking now (cleaning costs, regulations). Smoking in non-smoking car = $200-500 cleaning fee. If customer smokes, must do so outside vehicle. Low relevance today. |

---

## **System Preferences & Settings**

Global settings that affect how the system presents information and interacts with the user.

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Preferred Language** | Choice (BCP 47) | 20 chars | UI display language for Concur interface. Defaults to browser/system language if missing. | **Localization preference** - Customer sees interface in preferred language. If missing, system detects from browser (works for 90% of users). Helpful for multilingual users who want specific language regardless of location. Minor convenience. |
| **Country Code** | Choice | 2-letter ISO | Traveler's home country for default currency, tax calculations, policy assignment. | **System defaults** - Used for currency display, tax forms, regional policy application. If missing, system infers from company location or login IP. Mostly invisible to customer—backend setting. |
| **eReceipt Opt-In** | Yes/No | — | Receive electronic receipts via email instead of paper. Environmental preference and convenience. | **Paperless convenience** - Email receipts easier to organize than paper. Searchable, automatically archived. If missing, customer gets paper receipts (must scan manually for expense reports—extra work). eReceipts significantly faster for expense submission. Should arguably be "Recommended" for productivity. |

---

# **Behavioral Intelligence & Pattern Learning**

Beyond static preferences, Know Me Agent automatically learns behavioral patterns from user actions without manual configuration. This dynamic intelligence supplements explicit preferences and adapts over time.

---

## **Behavioral Intelligence & Pattern Learning**

The Know Me Agent automatically learns these preferences from user behavior without manual configuration:

### Learning Sources
- **Past bookings** - Flight seats, hotel brands, car types selected in previous trips
- **Expense submissions** - Meal spending patterns, parking locations, ground transport choices
- **Calendar events** - Travel frequency, typical destinations, booking lead times
- **Geo-location patterns** - Home airport, frequent destinations, regional preferences

### Pattern Detection Examples

**Flight Patterns:**
- Time of day: 80% of Fred's flights after 6pm → prefers evening departures
- Seat selection: 100% window seats → strong preference (97% confidence)
- Airlines: 90% United → loyalty-driven booking behavior
- Cabin class: 95% first class → willing to pay for comfort

**Hotel Patterns:**
- Brand loyalty: 22 years Marriott, 85% of stays → always prioritize Marriott (99% confidence)
- Room floor: 12 bookings requested high floor → consistent preference
- Property preference: 15 stays at Marriott Union Square → favorite property in SF

**Ground Transport Patterns:**
- Service provider: 70% Lyft vs 30% Uber → prefers Lyft
- Service tier: 60% Extra Comfort upgrades → willing to pay for space
- Pickup timing: Books rides 30 min before flight → buffer preference

**Dining Patterns:**
- Cuisine variety: Greek, Steakhouse, American, Vietnamese, Seafood → adventurous eater
- Solo vs. group: 80% solo dinners at 7pm → prefers dinner alone after work
- Restaurant type: Mid-to-high-end, no fast food → quality preference

**Expense Patterns:**
- Parking location: Always parks at PAE, flies from SEA → knows this saves money
- Meal costs: Average $45 dinner when solo, $120 when with clients → context-aware spending
- Receipt submission timing: Submits within 2 days of trip → organized traveler

### Confidence Scoring

Know Me Agent calculates confidence scores for each learned preference:

**Formula:**
```
Confidence = (Frequency × 40%) + (Consistency × 40%) + (Recency × 20%)
```

**Example: Fred's window seat preference**
- Frequency: 25 bookings / 25 total = 100% (40 points)
- Consistency: 25 out of 25 = 100% (40 points)
- Recency: Last booking 2 weeks ago = 90% (18 points)
- **Total Confidence: 98%**

**Confidence Levels:**
- **High confidence (>90%)**: "You always book window seats" → auto-apply
- **Medium confidence (70–90%)**: "You usually prefer Marriott" → suggest but don't auto-apply
- **Low confidence (<70%)**: "You sometimes book Lyft" → neutral, don't recommend

### Adaptive Learning

Know Me Agent detects pattern changes and adapts:
- **Shift detected**: Fred books 3 aisle seats after 25 window seats
- **Confidence adjustment**: 98% → 75%
- **User prompt**: "I noticed you've been booking aisle seats. Should I update your preference?"

### Preference Recommendations

After detecting patterns, Know Me proactively suggests saving them as explicit preferences:

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

---

## **Related Documentation**

- [Know Me Agent Main Requirements](../Profile-Know-Me-Agent-Requirements.md) - Feature 2: Dynamic Learning & Behavioral Pattern Recognition
- [Profile Validation Requirements](../Profile%20Validation/TPS-Field-Categories.md) - Field categories for booking validation
- [EA Assistant Integration](../EA-Assistant-Know-Me-Requirements.md) - Behavioral intelligence for EA Assistant use cases

---

## **Source**

Preference field definitions sourced from:
- Travel Profile Service (TPS) documentation
- Know Me Agent behavioral learning specifications
- Profile Know Me Agent Requirements - Section 5: Dynamic Learning & Behavioral Pattern Recognition
