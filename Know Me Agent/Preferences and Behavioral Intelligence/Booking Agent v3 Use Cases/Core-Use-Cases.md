# Booking Agent v3 - Know Me Integration: Core Use Cases

## Overview
This document outlines the 3 core use cases for integrating the Know Me Agent with the Concur Booking Agent v3. The integration follows a clear separation of concerns:

- **Know Me Agent**: Intelligence layer - surfaces user preferences, learned patterns, and behavioral intelligence
- **Booking Agent**: Presentation layer - displays options, handles user interaction, and completes bookings

---

## Use Case 1: Returning User with Trip History - Predictive Recommendations

### Scenario
A frequent business traveler opens Concur to book a flight from San Francisco to New York.

### Know Me Agent Responsibilities

**Static Preferences Provided:**
- Home Airport: SFO
- Air Seat Position: Aisle, Extra legroom
- Air Meal Preference: Vegetarian
- Hotel Room Type: King Bed
- Hotel Brand Preference: Hilton
- Car Type Preference: Mid-size, Automatic

**Learned Behavioral Patterns (Dynamic Intelligence):**

*Contextual Flight Preferences:*
- **Airline Loyalty**: United MileagePlus primary (High confidence 95%), but flies Delta for Atlanta routes (90% of ATL trips)
- **Trip purpose patterns**: Books premium economy for client meetings (85% confidence), economy for internal events (95% confidence)
- **Route-specific behaviors**: 
  - SFO→NYC: Prefers morning red-eye flights (saves hotel night), learned from 8 bookings
  - SFO→LAX: Prefers afternoon flights (avoids morning traffic), learned from 12 bookings
  - SFO→SEA: Always books direct even if $100 more expensive (5 consecutive direct bookings)
- **Layover intelligence**: Avoids connections <90 minutes (missed 2 connections historically), prefers direct flights when price difference <$150
- **Aircraft preferences**: Avoids Boeing 737 MAX on routes where alternatives exist (learned from 3 rebookings with Boeing avoidance pattern)
- **Cabin class flexibility**: Economy for flights <3hrs (98% confidence), Premium Economy for international >6hrs (92% confidence), upgrades to Business if <$300 difference on red-eyes

*Dynamic Hotel Intelligence:*
- **Destination-specific patterns**: 
  - NYC: Always books Midtown near Penn Station (12 stays), never downtown
  - SF: Prefers Union Square Marriott (15 past stays), but switches to Mission Bay Courtyard when meeting location is SoMa/biotech district
  - Chicago: Books airport hotels for <24hr trips (8 instances), downtown Loop area for multi-day stays (10 instances)
  - Austin: Prefers Downtown Hilton (6 stays) except during SXSW when books North Austin (learned from 2 festival-time trips)
- **Brand loyalty with flexibility**: 
  - Primary: Hilton family (65% of bookings)
  - Secondary: Marriott when Hilton unavailable or >30% more expensive
  - Independent boutique hotels when traveling to Portland/Seattle (emerging pattern, 4 occurrences)
- **Meeting proximity learning**: Books hotels within 0.5 miles of meeting location when multiple daily meetings detected in trip notes
- **Stay duration patterns**: 
  - Extended stay (>3 nights): Books hotels with kitchenettes, fitness center becomes priority filter
  - Short stay (<2 nights): Room quality prioritized over amenities
- **Floor & room preferences**: High floor preference (>10th floor) in urban hotels (78% confidence), ground floor for resort/suburban properties with parking access
- **Property-level learning**: Within Hilton brand, strongly prefers Garden Inn over Hampton Inn (learned from 8 upgrades/changes)

*Adaptive Ground Transport:*
- **City-specific transportation modes**: 
  - NYC/SF/Boston: Ride-share only (Uber/Lyft preferred), never rents cars (100% pattern consistency)
  - LA/Phoenix/Dallas: Always rents car (learned from 15 rentals vs 0 ride-shares)
  - Seattle: Rents car if trip >2 days, ride-share if day trip (contextual pattern)
  - Chicago: Uses ride-share for Loop/downtown, rents car for suburban office parks
- **Vehicle class adaptation**: 
  - Solo travel: Mid-size sedan standard (82% of bookings)
  - Traveling with colleagues: Upgrades to full-size or SUV (detected from shared expense reports, 6 instances)
  - Winter trips to mountain regions: Prefers AWD/SUV vehicles (4 occurrences)
- **Rental company preferences**: 
  - Primary: Enterprise (58% of rentals, proximity to hotels)
  - Secondary: Hertz at airports with dedicated lots (faster pickup)
  - Avoids: Budget/Economy tier brands (switched away 3 times)
- **Insurance & add-ons**: Consistently declines insurance and GPS (20+ rentals), but accepted GPS once in rural Montana (contextual exception)

*Booking Pattern Intelligence:*
- **Modification behavior**: 
  - Changes flights within 48hrs of booking 23% of time (indicates researching/comparing before finalizing)
  - Never modifies hotel once booked (high confidence in initial selection)
  - Cancels and rebooks car rentals when better rate appears (price-monitoring behavior detected)
- **Alternative preferences**: When primary choice unavailable, learned fallback sequence:
  - Flights: United → Alaska → Delta (never books Spirit/Frontier even when cheapest)
  - Hotels: Hilton Garden Inn → Courtyard Marriott → Hyatt Place
- **Multi-city intelligence**: Books separate one-way flights vs round-trip when visiting multiple cities (learned from 4 multi-city trips)

**Confidence Scores:**
- High (>90%) for frequently used patterns with consistency (e.g., airline loyalty, NYC hotel location)
- Medium (70-90%) for emerging patterns with 3-8 occurrences (e.g., boutique hotels in Portland)
- Low (<70%) for context-dependent or infrequent behaviors (e.g., AWD vehicles in winter)
- Adaptive: Confidence decreases if pattern breaks (e.g., switches from United to Delta 3x in a row triggers re-evaluation)

### Booking Agent Responsibilities

**Display Logic:**
- Flight search pre-filtered: United flights prioritized, aisle seats highlighted, morning departures sorted first
- Hotel results: Hilton properties at top with "Your preferred brand" badge, King bed rooms pre-selected
- Car rental: Mid-size automatic vehicles featured, insurance pre-declined
- Meal preferences: Vegetarian option auto-selected where available

**Visual Indicators:**
- Badges: "Your preferred airline", "Your usual hotel brand"
- Pre-selections: Seat type, meal, room type auto-filled
- Sorting: Results ordered by preference match score from Know Me

### User Benefit
Reduces booking time from 10+ minutes to 2-3 minutes with minimal clicks

### Success Metrics
- Average booking time reduction: Target 70% faster
- User satisfaction with pre-selections: Target >85%
- Override rate: Monitor <20% (indicates good prediction accuracy)

---

## Use Case 2: New User Onboarding - Contextual Preference Capture

### Scenario
A newly hired employee books their first business trip (flight + hotel).

### Know Me Agent Responsibilities

**Context Detection:**
- Identifies booking type: Flight + Hotel
- Detects user status: New user, no existing preferences
- Determines minimal required fields per context

**Just-in-Time Questionnaire Structure:**

**Stage 1 - Flight Preferences (30 seconds):**
1. Home Airport: [Dropdown] *(Required)*
2. TSA PreCheck/Global Entry? [Yes/No toggle] *(Required)*
3. Airline loyalty programs? [Optional - Skip available]

**Stage 2 - Hotel Preferences (20 seconds):**
1. Preferred Room Type: [King/Queen/Double/Twin] *(Required)*
2. Hotel Brand Preference: [Multi-select: Hilton/Marriott/Hyatt/IHG/No Preference] *(Required)*
3. Special needs? [Optional - Skip available]

**Progressive Profiling Strategy:**
- Stores only collected preferences (4 required fields total)
- Learns everything else from actual booking behavior
- Sets explicit preferences to 100% confidence immediately

**Behavioral Learning (First Booking):**
- Observes which seat position user selects → learns seat preference
- Tracks which amenities user filters by → learns hotel amenities preference
- Notes which flight times user chooses → learns time-of-day preference
- Records declined add-ons → learns cost-consciousness patterns

### Booking Agent Responsibilities

**Display Logic:**
- Contextual prompt: "Quick setup (1 minute) - Let's capture your travel preferences"
- Staged collection: Show Stage 1 → Apply → Show Stage 2 → Apply
- "Skip for now" option for all optional fields
- Immediately applies captured preferences to current search results

**Onboarding UX:**
- Progress indicator: "Step 1 of 2"
- Simple selection UI: Dropdowns, toggles, multi-select chips
- No text entry required for initial setup
- Clear value proposition: "This will help us personalize your search"

### User Benefit
Minimal 1-minute setup gets them booking immediately, while system learns nuanced preferences from behavior

### Success Metrics
- Onboarding completion rate: Target >90%
- Time to complete onboarding: Target <90 seconds
- Preference capture rate: Target 4 required fields + 3-5 learned behaviors per first booking
- Second booking speed improvement: Target >50% faster than first booking

---

## Use Case 3: Routine Route Traveler - One-Click Rebooking

### Scenario
A regional sales manager travels from Chicago to Detroit every month for recurring client meetings.

### Know Me Agent Responsibilities

**Pattern Detection Algorithm:**
Uses: Frequency × 40% + Consistency × 40% + Recency × 20%

**Detected Patterns (High Confidence 95%):**
- **Recurring Route**: ORD → DTW, monthly frequency
- **Flight Bundle**: 
  - Outbound: United UA 645, Monday 7:00 AM
  - Return: United UA 892, Wednesday 6:30 PM
  - Seat: 12A (aisle, exit row) consistently selected
- **Hotel Property**: Marriott Renaissance Downtown Detroit (property ID: 12345)
  - Room Details: King bed, non-smoking, 14th floor or higher
  - Special Request: Always adds "early check-in"
- **Car Rental**: Enterprise, Mid-size sedan, Automatic
  - Declined Add-Ons: GPS, insurance, breakfast (knows it's included)
- **Booking Lead Time**: Always books 2 weeks in advance

**Behavioral Intelligence Provided:**
- Preferred Flight Times: Morning outbound (7-9 AM), evening return (6-8 PM)
- Airline Loyalty: United (95% of bookings on this route)
- Hotel Floor Preference: 14th floor or higher (learned from past requests)
- Cost Optimization: Declines optional add-ons consistently

**Complete Travel Bundle:**
Packaged as single atomic unit: Flight + Hotel + Car with all preferences pre-configured

### Booking Agent Responsibilities

**Display Logic:**

**Smart Prompt Card:**
```
┌─────────────────────────────────────────────┐
│ Book your usual Chicago → Detroit trip?    │
│                                             │
│ ✈️  UA 645  Mon 7:00 AM → Wed 6:30 PM      │
│     Seat 12A (Exit row, Aisle)             │
│                                             │
│ 🏨  Marriott Renaissance Downtown          │
│     King bed, High floor, Early check-in   │
│                                             │
│ 🚗  Enterprise Mid-size Automatic          │
│                                             │
│ 💰  Total: $1,247 (typical range)          │
│                                             │
│ [Book This Trip] [Customize] [Start Fresh] │
└─────────────────────────────────────────────┘
```

**Intelligent Fallback Scenarios:**

1. **If preferred flight unavailable:**
   - "Your usual flight (UA 645) is full. Book UA 712 (8:15 AM) instead? [Your next typical choice]"
   - Shows next-best match based on learned time preferences

2. **If hotel rate anomaly detected:**
   - "⚠️ Marriott is $95 higher than usual ($189 vs $94 avg). See alternatives?"
   - Provides comparison with similar properties

3. **If preferred seat taken:**
   - Auto-suggests: 12C (same row, aisle) or 11A (nearby exit row)
   - Shows reasoning: "Similar to your usual 12A"

**One-Click Confirmation:**
- Single button books entire package
- Confirmation page shows all details for final review
- Email confirmation includes all booking references

### User Benefit
15-minute multi-component booking reduced to 5-second confirmation; maintains control with intelligent alternatives

### Success Metrics
- Pattern detection accuracy: Target >90% for routes with 3+ bookings
- One-click acceptance rate: Target >80% for high-confidence routes
- Fallback option acceptance: Target >70% when primary unavailable
- Time savings: Target >90% reduction in booking time for routine trips

---

## Confidence Score Application Guidelines

### High Confidence (>90%)
- **Know Me**: Provides as primary recommendation
- **Booking Agent**: Auto-selects, pre-fills, sorts to top
- **Example**: One-click booking for routine routes

### Medium Confidence (70-90%)
- **Know Me**: Provides as suggested option
- **Booking Agent**: Highlights with badges, secondary sorting
- **Example**: "Your preferred airline" badge on United flights

### Low Confidence (<70%)
- **Know Me**: Provides as background context
- **Booking Agent**: Subtle hints, tertiary sorting
- **Example**: Recent one-time behavior not yet established as pattern

---

## Required Preferences by Booking Context

### Flight Booking
**Required (2 fields - 30 seconds):**
- Home Airport (IATA code)
- TSA PreCheck/Global Entry status

**Optional (Learned from behavior):**
- Air Seat Position
- Air Meal Preference
- Airline Loyalty Programs
- Baggage allowance needs
- Refundability preference
- Cabin Class Preference

### Hotel Booking
**Required (2 fields - 20 seconds):**
- Hotel Room Type (King/Queen/Double/Twin)
- Hotel Brand Preference

**Optional (Learned from behavior):**
- Hotel Smoking Preference
- Hotel Amenities (Gym, Pool, Restaurant, Room Service)
- Rate Eligibility (AAA, AARP, Government, Military)
- Special requests (Pillows, Crib, Rollaway, Early Check-In)
- Accessibility Needs

### Car Rental Booking
**Required (1 field - 10 seconds):**
- Car Transmission (Automatic/Manual)

**Optional (Learned from behavior):**
- Car Type Preference (Economy, Compact, Mid-size, Full-size, SUV)
- Car Rental Add-Ons (GPS, Ski Rack, Smoking)
- Rental provider loyalty programs

### Rail Booking
**Required (0-1 fields - 10 seconds):**
- BahnCard Type & Class (if applicable)

**Optional (Learned from behavior):**
- Rail Seat preference
- Rail Coach Type
- Rail Noise Comfort

---

## Key Performance Indicators (KPIs)

### Overall Integration Success
- **Booking Time Reduction**: 60% average decrease across all users
- **User Satisfaction**: >85% satisfaction with personalized recommendations
- **Adoption Rate**: >70% of users complete preference onboarding
- **Learning Velocity**: 3 bookings to establish high-confidence patterns

### Use Case Specific Metrics

**UC1 - Returning User:**
- Preference match accuracy: >85%
- Override rate: <20%
- Time savings: 70% reduction

**UC2 - New User:**
- Onboarding completion: >90%
- Time to first booking: <5 minutes from account creation
- Second booking improvement: >50% faster

**UC3 - Routine Traveler:**
- Pattern detection accuracy: >90%
- One-click usage: >80% for qualifying routes
- Fallback acceptance: >70%

---

## Future Enhancement Opportunities

### Enhanced Context Detection
- Meeting context (client vs internal) affects upgrade preferences
- Trip duration impacts hotel amenity importance
- Destination type (urban vs resort) changes car rental needs

### Collaborative Filtering
- "Travelers like you also prefer..."
- Corporate travel policy optimization
- Team travel pattern learning

### Predictive Booking
- Proactive trip suggestions based on calendar
- "You usually book Detroit 2 weeks before the monthly client review"
- Budget forecasting for upcoming trips

### Multi-Modal Learning
- Email parsing for trip context
- Calendar integration for meeting importance
- Expense report analysis for cost patterns

---

## Version History
- v1.0 - Initial core use cases defined (2026-05-13)
