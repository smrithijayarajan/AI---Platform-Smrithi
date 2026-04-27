# Travel Profile Service (TPS) Field Categories

Based on Travel Profile Service field details, this document categorizes profile fields by their importance for booking scenarios.

**Note:** Travel preferences, rate preferences, rail preferences, hotel amenities, car rental add-ons, and behavioral intelligence have been moved to:
→ [Preferences and Behavioral Intelligence](../Preferences%20and%20Behavioral%20Intelligence/Preference-Fields-and-Learning.md)

This document focuses on:
- ✅ **Required fields** - Identity, travel documents (passport, visa)
- ✅ **Critical non-blocking fields** - Contact information (email, phone)
- ✅ **Recommended fields** - TSA/Trusted Traveler, loyalty programs, emergency contacts
- ✅ **Optional fields** - Personal information, work/organization, additional documents

---

## **REQUIRED Fields for Booking**

### Identity & Legal Name (All Bookings)

| Field Name | Type | Max Length | Why Required | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Travel CRS Name** | Text | 60 chars | Must match traveler's ID/passport exactly for GDS booking. Format: LASTNAME/FIRSTNAME | **Booking failure** if missing or mismatched. Airlines reject tickets when name doesn't match ID. Customer cannot board flight and loses money on non-refundable tickets. |
| **First Name** | Text | 60 chars | Legal identity verification required by all travel suppliers (airlines, hotels, car rentals) | **Booking rejection** - Cannot complete any reservation without legal first name. System cannot generate ticket or confirmation. |
| **Last Name** | Text | 60 chars | Legal identity verification required by all travel suppliers | **Booking rejection** - Cannot complete any reservation without legal last name. System cannot generate ticket or confirmation. |

### International Travel - REQUIRED
# Smrithi + Daniel's comments - Check Module Property ( Profile Wizard API) and move these to recommended

| Field Name | Type | Max Length | Why Required | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Passport Number** | Text | 100 chars | Required by immigration authorities for international border crossing. Airlines must transmit passenger data (APIS - Advanced Passenger Information System) before departure. | **Booking blocked** - International flights cannot be ticketed without passport number. Customer forced to manually add at check-in, causing delays. Risk of denied boarding if not added before departure. |
| **Passport Issuing Country** | Choice | 2-letter ISO | Immigration systems validate passport authenticity against issuing country. Required for visa eligibility checks. | **Immigration denial** - Without this, airline cannot validate visa requirements. Customer may be denied boarding if destination country requires visa for their passport country. |
| **Passport Nationality** | Choice | 2-letter ISO | Determines visa requirements, visa-waiver eligibility (e.g., US ESTA), and entry permissions for destination country. | **Entry denial at border** - Customer may arrive at destination and be denied entry if visa-waiver assumptions were incorrect. Airline may deny boarding to avoid fines. |
| **Passport Expiration Date** | Date | YYYY-MM-DD | Most countries require passport validity 6+ months beyond travel dates. Airlines check this before boarding to avoid fines for transporting passengers who will be denied entry. | **Denied boarding** - Customer arrives at airport and is turned away if passport expires too soon. Loses entire trip cost. No rebooking possible without passport renewal. |
| **Middle Name** | Text | 60 chars | Required by some airlines (especially for international flights) to match passport exactly. US TSA Secure Flight program requires exact name match. | **Booking rejection or flight delay** - If name on ticket doesn't match passport middle name, customer faces re-ticketing fees ($200+) or denied boarding. Must arrive extra early to resolve at airport. |

---

## **RECOMMENDED Fields for Enhanced Experience**

### Contact Information ⚠️ CRITICAL - Do Not Block Booking

**Note:** While these fields don't block booking completion, they are **critical for travel success**. Missing contact information means customers miss flight delays, gate changes, and cancellations—causing missed connections, stranded travelers, and ruined trips. Strongly prompt for these fields before finalizing bookings.

| Field Name | Type | Max Length | Why Critical | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Primary Email Address** | Text (ASCII) | 255 chars | **ALL booking confirmations, itinerary changes, boarding passes, hotel confirmations sent via email.** Only reliable way to receive critical travel updates and documents. Airlines send last-minute gate changes, delay notifications, cancellation alerts via email. | **⚠️ CRITICAL: Customer misses flight delays/cancellations** - Without email, customer unaware of 3-hour delay until airport arrival (wasted trip). Misses cancellation notice—shows up at airport for non-existent flight. Cannot access mobile boarding pass—forced to print at kiosk (long lines, risk of missing flight if check-in deadline passed). Must call customer service (45+ min wait) to get confirmation numbers. **Cannot receive itinerary if plans change—stranded without hotel address or confirmation codes.** |
| **Primary Mobile Phone Number** | Text | 60 chars | **Real-time SMS alerts for flight delays (average 45 min), cancellations, gate changes (5-10 min notice), boarding calls.** Enables TextApp receipt submission (eliminates paper receipts, saves 30+ min per expense report). Hotel/ride-share coordination via SMS. Duty of care emergency contact during travel. | **⚠️ CRITICAL: Customer misses time-sensitive alerts** - Flight delayed 2 hours but customer doesn't know—arrives at airport 2 hours early unnecessarily OR misses earlier notification and arrives for original time (gate already closed). **Gate change from B12 to C45 with 10 min notice—customer boards wrong flight or misses flight entirely.** Cancellation notification via SMS at 6 AM—customer without phone sleeps through, drives to airport for cancelled flight. Cannot use TextApp—must save paper receipts for weeks, manual upload takes 45+ min. Ride-share driver cannot reach customer for pickup (wait time exceeded, ride cancelled, customer stranded). **In emergency, company cannot reach traveler for duty of care (natural disaster, political unrest, medical emergency).** |
| **Phone Country Code** | Choice | 2-letter ISO | Ensures SMS/calls reach correct international phone number format (+1 US, +44 UK, +49 DE). Without correct country code, all notifications sent to wrong number. | **⚠️ CRITICAL: All communication fails internationally** - Airline sends gate change SMS to +1 (assuming US) but customer's phone is +44 UK—message never received. Customer misses gate change, flight departs without them. Hotel cannot reach customer for late arrival—cancels room as no-show. Ride-share driver calls wrong number—customer stranded at airport. Emergency contact attempts fail—family cannot reach traveler in crisis. |

### TSA/Trusted Traveler (US Travel)

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **TSA PreCheck Number / Known Traveler Number (KTN)** | Text | 150 chars | Enables expedited security screening at US airports. Separate, faster TSA PreCheck lanes skip removing shoes, laptops, liquids, belts. | **Time savings: 15-30 minutes per flight** - Customer waits in long standard security line (45+ min during peak times) vs. TSA PreCheck line (5-10 min). Reduces airport arrival time, lowers stress, minimizes missed flight risk. Without KTN, customer loses benefit they paid for ($85/5 years). |
| **Redress Number** | Text | 150 chars | For travelers mistakenly flagged on government watchlists (name similarity to restricted persons). Prevents repeated "SSSS" stamps causing extra screening. | **Eliminates harassment and delays** - Without redress number, customer faces extra 30-60 min screening at every checkpoint, invasive bag searches, interrogation. Causes missed flights and extreme stress. Redress number clears false match permanently. |
| **TSA Date of Birth** | Date | YYYY-MM-DD | TSA Secure Flight program matches passengers against watchlists using name + DOB. Reduces false positives (common names). | **Prevents security delays** - Without DOB, customer with common name (e.g., "John Smith") flagged for extra screening due to name match. Adding DOB disambiguates identity, eliminates unnecessary delays. |
| **TSA Gender** | Choice | — | Required for TSA Secure Flight data matching. Must match ID used for travel. | **Avoids TSA mismatch errors** - If ticket gender doesn't match ID, customer flagged at security for manual verification. Causes delays and uncomfortable questioning. |

### Loyalty Programs (Earn Points/Miles)

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **Air Loyalty Program Account Number** | Text | 60 chars | Automatically earns frequent flyer miles for every flight booked. Miles accumulate toward free flights, upgrades, elite status. | **Lost value: $200-500+ per trip** - Without loyalty number, customer loses miles worth hundreds of dollars. Elite status benefits (free bags, upgrades, priority boarding) unavailable. Cannot retroactively claim miles easily—requires manual forms and receipts. |
| **Air Loyalty Vendor Code** | Text (ASCII) | 2 chars | Links correct frequent flyer program to airline (e.g., UA for United, AA for American). Ensures miles credit to right account. | **Miles credited to wrong program or lost** - Customer books United flight but miles go to wrong airline. Miles lost forever or require complex transfer process. Cannot reach elite status thresholds. |
| **Hotel Loyalty Account Number** | Text | 60 chars | Earns hotel points (Marriott Bonvoy, Hilton Honors) for every stay. Points redeem for free nights, upgrades. Elite status provides free breakfast, room upgrades, late checkout. | **Lost value: $100-300+ per stay** - Customer loses free night credits, upgrades, elite benefits (free breakfast worth $30/day). Must pay for upgrades that would be free with status. Cannot accumulate points toward vacation stays. |
| **Car Rental Loyalty Account Number** | Text | 60 chars | Earns rental credits, enables faster pickup (skip counter line), provides upgrade eligibility, and insurance benefits. | **Lost time and perks** - Customer waits in 20+ min line at rental counter. Loses free upgrade opportunities. Must fill out paperwork every time. No loyalty discounts or insurance benefits applied. |
| **Rail Loyalty Account Number** | Text | 60 chars | For frequent rail travelers (Amtrak, European rail operators). Earns points toward free trips, discounts, priority booking. | **Lost rewards: $50-200+ per trip** - Customer loses points/discounts on frequent rail travel. Must pay full price for future trips instead of redeeming free travel earned through loyalty. |

### International Travel - RECOMMENDED

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **Visa Number** | Text | 100 chars | Required for entry to countries that mandate visas (China, India, Russia, Brazil, etc.). Must be obtained weeks/months before travel. | **Entry denied at border** - Customer arrives at destination without visa, denied entry by immigration. Deported on next flight at customer's expense ($1,000+ last-minute ticket). Entire trip cancelled, hotels/meetings lost. Visa application takes weeks—missing visa number means Know Me Agent cannot warn in time to apply. |
| **Visa Type** | Choice | — | Different visas allow different activities (tourist vs. business vs. multi-entry). Wrong visa type causes entry denial or restrictions. | **Entry complications or denial** - Customer enters on tourist visa but conducts business (illegal in many countries). Immigration discovers work emails/materials, deports traveler and bans future entry. Single-entry visa used up on first leg, cannot re-enter for return flight. Multi-entry needed for trips with multiple countries. |
| **Visa Expiration Date** | Date | YYYY-MM-DD | Airlines check visa validity before boarding. Expired visa causes denied boarding and trip cancellation. | **Denied boarding at departure** - Customer arrives at airport, airline sees expired visa, denies boarding (airline fined if transporting invalid passengers). Customer loses entire trip cost (flight, hotels, meetings). No time to renew visa—process takes weeks. Trip cancelled, must rebook after renewal ($3,000+ total loss). |
| **Visa Country Issued** | Choice | 2-letter ISO | Some visas valid only for specific entry points or issued by specific countries. Validates visa authenticity. | **Immigration delays or denial** - Visa issued by wrong country/embassy causes confusion at border. Customer detained for questioning (30-120 min). May be denied entry if visa authenticity questioned. Ruins arrival experience and causes missed connections/meetings. |

### Emergency & Safety

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **Emergency Contact Name** | Text | 255 chars | Airlines, hotels, and hospitals need emergency contact if traveler is incapacitated (medical emergency, accident, severe illness). | **No one notified in crisis** - Customer has medical emergency abroad (heart attack, accident). Hospital cannot reach family for 12+ hours. Family unaware of crisis, cannot make medical decisions or travel to bedside. Extreme distress for family and delayed critical care decisions. |
| **Emergency Contact Phone** | List | — | Enables immediate notification if traveler is hospitalized, injured, or in life-threatening situation. | **Family unreachable in emergency** - Hospital tries wrong number or no contact info available. Family doesn't learn of emergency until traveler regains consciousness (days later). Cannot provide medical history, insurance info, or consent for procedures. Legal/medical complications. |
| **Emergency Contact Relationship** | Choice | — | Clarifies authority for medical decisions (spouse has legal authority, siblings may not). Hospitals prioritize contacts based on relationship. | **Medical decision delays** - Hospital unsure who has legal authority for emergency surgery. Delays treatment while verifying relationship/authority. Critical minutes lost in life-threatening situation. Wrong person contacted first (distant relative instead of spouse). |
| **Medical Alerts** | Text | 3 chars (sync) | Alerts airlines/hotels to critical medical conditions (allergies, diabetes, heart conditions) for emergency response. *Note: Only 3 chars preserved in backend—severely limited utility.* | **Emergency responders unaware of condition** - Customer has severe allergic reaction, EpiPen in bag. Airline crew unaware of allergy, delays treatment. Diabetic customer loses consciousness, crew doesn't know to check blood sugar. Life-threatening delays in proper care. **Technical limitation makes this field nearly useless—only 3 characters sync to booking systems.** |

---

## **OPTIONAL Fields (Nice-to-Have)**

### Personal Information

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Name Prefix** | Text | 60 chars | Courtesy title (Mr., Mrs., Dr., Prof.) for professional/formal communications. Not required for travel bookings. | **Minimal impact** - Used only for formal correspondence. Missing prefix doesn't affect booking functionality. Some customers prefer no title for privacy/gender reasons. Purely presentational—no operational impact. |
| **Middle Name** | Text | 60 chars | Optional for domestic travel. Only required for international flights by some airlines. | **No impact for domestic travel** - Domestic bookings work fine without middle name. For international, if required, it becomes mandatory (covered in Required section). Having it pre-filled saves time but not critical for most trips. |
| **Name Suffix** | Text | 60 chars | Generational/honorific suffix (Jr., Sr., III, Esq.). Rarely required for bookings. | **Minimal impact** - Used for formal communication and name disambiguation in large companies. Booking systems rarely require it. Missing suffix doesn't affect ticket validity or travel experience. |
| **Preferred Name** | Text | 60 chars | Display name for internal systems only. Never appears on tickets or legal documents. | **Internal UX only** - Makes UI more personalized ("Welcome back, Mike!" vs. "Welcome back, Michael!"). No impact on booking functionality. Purely cosmetic for user comfort. |
| **Date of Birth** | Date | YYYY-MM-DD | Used for age-based discounts (senior, youth), loyalty program enrollment, and some international destinations. | **Minor convenience** - Enables automatic senior discounts (AARP rates), youth hostel eligibility. Some hotels/rentals verify age at check-in (25+ for car rentals). Helpful but workarounds exist (provide at booking time). |
| **Gender** | Choice | — | Optional personal data. Some loyalty programs track demographics. Hotels may use for room assignment preferences. | **Minimal impact** - Rarely affects booking. Some cultures/hotels have gender-specific floors or facilities. Demographic data for company travel analytics. Customer can skip if prefer not to disclose. |

### Work/Organization

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Job Title** | Text | 255 chars | Used for company reporting, travel policy enforcement, VIP handling. Not required for booking functionality. | **Internal reporting only** - Helps travel managers analyze travel by role (executives vs. ICs). Some hotels provide VIP treatment for C-level titles. No impact on booking capability. Useful for expense categorization but not mandatory. |
| **Company Employee ID** | Text | 48 chars | Links travel profile to HR system for expense reconciliation and policy enforcement. Internal identifier only. | **Backend convenience** - Streamlines expense report matching to employee records. No customer-facing impact. Customer can book travel without it—expense team reconciles manually if needed (slightly slower). |
| **Cost Center** | Text | 25 chars | Accounting code for billing travel expenses to correct department budget. Used in expense reporting, not booking. | **Finance reporting only** - Ensures travel costs charged to correct budget. Doesn't affect booking or travel experience. Finance team can re-code expenses if missing (manual process but not blocking). |
| **Rule Class** | Text | 60 chars | Determines travel policy tier (executive policy vs. standard). Affects what the customer is allowed to book (class of service, hotel price limits). | **Policy enforcement** - Without rule class, system may default to most restrictive policy (denies premium options). Or defaults to most permissive (expense rejection later). Affects comfort and convenience but not absolute ability to book—can override with manager approval. |

### Additional Contact

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Home Address** | Address | Various | Used for car rental pickups at home, emergency contact purposes, and mileage reimbursement calculations. | **Convenience feature** - Enables "pick up at home" for car services. Helps calculate distance for mileage reimbursement. Not required for bookings—customer can enter ad-hoc if needed. Minor time savings only. |
| **Work Address** | Address | Various | Used for default pickup/drop-off location for ground transportation, meeting location coordination. | **Convenience feature** - Pre-fills ride pickup at office. Not critical—customer can enter address manually during ride booking. Saves 30 seconds. No impact if missing. |
| **Secondary Email Addresses** | Text | 255 chars | Backup email for confirmations. Useful if primary email account has issues or customer wants work+personal copies. | **Redundancy only** - Primary email sufficient for 99% of travel needs. Secondary useful if primary goes down (rare). Customer can add CC address during booking if needed. Nice-to-have redundancy but not necessary. |
| **Additional Phone Numbers** | Text | 60 chars | Work phone, home phone, fax (legacy). Backup contact methods if primary mobile unavailable. | **Legacy/redundancy** - Most communication now via mobile. Work phone rarely needed (calls go to mobile). Fax obsolete. Pager obsolete. Minimal value—primary mobile handles 99% of contact needs. |

### Additional Travel Documents

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Driver's License Number** | Text | 200 chars | Required only for car rentals. Can be provided at rental counter if missing from profile. | **Car rental convenience** - Pre-fills driver's license at booking, saves 1 minute. If missing, customer provides at rental counter (standard process). No blocking impact—just slower counter experience. |
| **Driver's License Issuing Country/State** | Choice/Text | 2/30 chars | Validates license legitimacy for international car rentals. Some countries don't accept licenses from others. | **International rental validation** - Helps detect if International Driving Permit (IDP) needed. If missing, customer may discover at counter (delays pickup 30+ min while getting IDP). Rare issue—US/EU licenses widely accepted. |
| **Driver's License Expiration Date** | Date | YYYY-MM-DD | Car rental companies reject expired licenses. Pre-validation prevents counter rejection. | **Rental denial prevention** - If license expired, car rental denies vehicle (customer stranded). Proactive check saves trip failure but customer can also check manually. Helpful but not critical for most users. |
| **National ID Document Number** | Text | 200 chars | Alternative to passport for some domestic/regional travel (EU internal flights, domestic Brazil, etc.). | **Regional convenience** - EU citizens can use national ID instead of passport for Schengen travel (lighter, easier). If missing, passport still works—no blocking impact. Convenience for frequent regional travelers only. |

### Payment Card Defaults

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Default Card for Air** | Yes/No | — | Pre-selects preferred payment card for flight bookings. Prevents accidental use of wrong card. | **Convenience** - Saves selecting card from dropdown every time. If missing, customer selects card manually (5 seconds). Helpful for frequent travelers with multiple cards but low impact. Note: Payment card data itself is managed separately by payment systems, not stored in travel profile. |
| **Default Card for Car** | Yes/No | — | Pre-selects preferred card for car rentals. | **Convenience** - Auto-fills payment selection at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Hotel** | Yes/No | — | Pre-selects preferred card for hotel bookings. | **Convenience** - Auto-fills payment selection at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Rail** | Yes/No | — | Pre-selects preferred card for rail bookings. | **Convenience** - Auto-fills payment selection at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Taxi** | Yes/No | — | Pre-selects preferred card for ground transportation and ride-shares. | **Convenience** - Auto-fills payment selection at checkout. If missing, customer selects manually. Minor time savings only. Ride apps (Uber/Lyft) have own payment methods. |

## **Summary by Booking Scenario**

### Domestic Flight (US)
**Required:** Travel CRS Name, First Name, Last Name
**Critical (Non-Blocking):** Email, Mobile Phone
**Recommended:** TSA PreCheck KTN, Airline Loyalty Number, Seat Preferences
**Optional:** Meal Preference, Special Assistance

### International Flight
**Required:** Travel CRS Name, First Name, Last Name, Middle Name, Passport Number, Passport Expiration, Passport Issuing Country, Nationality
**Critical (Non-Blocking):** Email, Mobile Phone, Phone Country Code
**Recommended:** Visa (if applicable), TSA PreCheck, Airline Loyalty
**Optional:** Emergency Contact, Medical Alerts

### Hotel Booking
**Required:** First Name, Last Name
**Critical (Non-Blocking):** Email, Mobile Phone
**Recommended:** Hotel Loyalty Number, Room Type Preference, Smoking Preference
**Optional:** Amenity Preferences (gym, pool, crib, etc.)

### Car Rental
**Required:** First Name, Last Name, Driver's License Number
**Critical (Non-Blocking):** Email, Mobile Phone
**Recommended:** Car Loyalty Number, Car Type Preference, Transmission
**Optional:** GPS, Ski Rack

### Rail Booking (Europe)
**Required:** First Name, Last Name
**Critical (Non-Blocking):** Email, Mobile Phone
**Recommended:** Rail Loyalty Number, BahnCard (for Deutsche Bahn), Seat Preferences
**Optional:** Coach Type, Meal Preference, Special Assistance

---

## **Validation Rules for Know Me Agent**

### Blocking Validations (Prevent Booking)
- International flight without passport number/expiration
- Missing Travel CRS Name or legal name fields

### Critical Non-Blocking Validations (Allow Booking, Strong Warning) ⚠️
**Contact Information - Don't block booking but show prominent warning:**
- Missing primary email address - Customer will not receive confirmations, boarding passes, or flight alerts
- Missing primary mobile phone number - Customer will miss gate changes, delays, cancellations via SMS
- Missing phone country code - International travelers will not receive any SMS notifications

**Rationale:** We don't want to block a booking completely, but missing contact info significantly increases risk of:
- Missed flights due to gate changes (average 10 min notice)
- Arriving at airport unaware of cancellations (wasted trip)
- Cannot access mobile boarding pass (forced to use kiosk, risk missing check-in deadline)
- Stranded during travel without way to receive updated itinerary

**UX Pattern:** Multi-step warning before allowing skip:
1. First prompt: "I strongly recommend adding contact info - here's why..."
2. If user skips: "⚠️ Final warning - without contact info you risk missing your flight. Add now?"
3. If user skips again: Allow booking but log high-priority work item for follow-up

### Warning Validations (Allow Booking, Show Warning)
- Passport expires within 6 months of travel date (many countries require 6-month validity)
- Visa expiration before return date
- Driver's license expired for car rental

### Recommended Validations (Suggest but Don't Block)
- Missing TSA PreCheck for US domestic flights (suggest adding to save time)
- Missing airline loyalty number (suggest adding to earn miles)
- Missing hotel loyalty number (suggest adding to earn points)
- Missing visa for international destinations that commonly require visas

### Contextual Validation Examples

**Scenario: Fred books flight to London (international)**
```
Know Me validates:
✓ Travel CRS Name: FREDERICKS/FRED
✓ Passport Number: X12345678
✓ Passport Nationality: US
✓ Passport Issuing Country: US
⚠ Passport Expiration: 2026-08-15 (expires in 4 months - warn about 6-month rule)
✗ Middle Name: Missing (required by airline for international - BLOCK)

Prompt: "I need your middle name to complete this international booking. Airlines require it to match your passport exactly."
```

**Scenario: Fred books domestic US flight**
```
Know Me validates:
✓ Travel CRS Name: FREDERICKS/FRED
⚠ TSA PreCheck: Not enrolled (recommend)
⚠️ CRITICAL - Email: Missing (strongly recommend - cannot receive alerts)
⚠️ CRITICAL - Mobile Phone: Missing (strongly recommend - cannot receive gate changes)

Prompt: "Before I finalize your booking, I strongly recommend adding your contact information:

📧 Email address - I'll send your boarding pass and any flight updates here
📱 Mobile phone - You'll get instant SMS alerts if your gate changes or flight is delayed

Without these, you might miss critical updates and could miss your flight. It only takes 30 seconds to add them.

Would you like to add your contact info now, or proceed without it? (Not recommended)"
```

**Scenario: Fred books international flight without contact info**
```
Know Me validates:
✓ All required fields present (passport, legal name)
⚠️ CRITICAL - Email: Missing
⚠️ CRITICAL - Mobile Phone: Missing

Prompt: "⚠️ Important: Your booking is ready, but you're missing critical contact information.

Without an email address and mobile phone number:
• You won't receive your boarding pass
• You'll miss gate changes (average 10 minutes notice)
• You won't be notified if your flight is delayed or cancelled
• You cannot use our TextApp to submit receipts during your trip
• We cannot reach you in an emergency

This significantly increases your risk of missing your flight or being stranded during travel.

I can add this information in 30 seconds. What's your email and mobile number?"

[Allow skip with additional warning: "I understand the risks - proceed without contact info"]
```

---

## **Key Technical Notes**

- **Medical Alerts**: Only first 3 characters preserved in backend sync (effectively unusable for detailed medical information)
- **National ID Number**: Truncated at 200 characters during sync
- **Total Profile Size Limit**: 400 KB per profile (AWS DynamoDB cap)
- **ASCII-only fields**: Travel CRS Name, Email, Vendor Codes, Country Codes, Phone Number, Card Number

---

## **Source**

Field definitions sourced from Travel Profile Service (TPS) internal documentation:
`https://pages.github.concur.com/travel-profile/docs/Travel%20Profile%20Service%20%28TPS%29/field-details/`
