# Travel Profile Service (TPS) Field Categories

Based on Travel Profile Service field details, this document categorizes profile fields by their importance for booking scenarios.

---

## **REQUIRED Fields for Booking**

### Identity & Legal Name (All Bookings)

| Field Name | Type | Max Length | Why Required | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Travel CRS Name** | Text | 60 chars | Must match traveler's ID/passport exactly for GDS booking. Format: LASTNAME/FIRSTNAME | **Booking failure** if missing or mismatched. Airlines reject tickets when name doesn't match ID. Customer cannot board flight and loses money on non-refundable tickets. |
| **First Name** | Text | 60 chars | Legal identity verification required by all travel suppliers (airlines, hotels, car rentals) | **Booking rejection** - Cannot complete any reservation without legal first name. System cannot generate ticket or confirmation. |
| **Last Name** | Text | 60 chars | Legal identity verification required by all travel suppliers | **Booking rejection** - Cannot complete any reservation without legal last name. System cannot generate ticket or confirmation. |

### International Travel - REQUIRED

| Field Name | Type | Max Length | Why Required | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Passport Number** | Text | 100 chars | Required by immigration authorities for international border crossing. Airlines must transmit passenger data (APIS - Advanced Passenger Information System) before departure. | **Booking blocked** - International flights cannot be ticketed without passport number. Customer forced to manually add at check-in, causing delays. Risk of denied boarding if not added before departure. |
| **Passport Issuing Country** | Choice | 2-letter ISO | Immigration systems validate passport authenticity against issuing country. Required for visa eligibility checks. | **Immigration denial** - Without this, airline cannot validate visa requirements. Customer may be denied boarding if destination country requires visa for their passport country. |
| **Passport Nationality** | Choice | 2-letter ISO | Determines visa requirements, visa-waiver eligibility (e.g., US ESTA), and entry permissions for destination country. | **Entry denial at border** - Customer may arrive at destination and be denied entry if visa-waiver assumptions were incorrect. Airline may deny boarding to avoid fines. |
| **Passport Expiration Date** | Date | YYYY-MM-DD | Most countries require passport validity 6+ months beyond travel dates. Airlines check this before boarding to avoid fines for transporting passengers who will be denied entry. | **Denied boarding** - Customer arrives at airport and is turned away if passport expires too soon. Loses entire trip cost. No rebooking possible without passport renewal. |
| **Middle Name** | Text | 60 chars | Required by some airlines (especially for international flights) to match passport exactly. US TSA Secure Flight program requires exact name match. | **Booking rejection or flight delay** - If name on ticket doesn't match passport middle name, customer faces re-ticketing fees ($200+) or denied boarding. Must arrive extra early to resolve at airport. |

### Payment - REQUIRED

| Field Name | Type | Max Length | Why Required | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Credit Card Account Number** | Text (ASCII) | 150 chars | All travel bookings require payment guarantee. Hotels/car rentals hold deposit. Airlines charge at time of booking. | **Complete booking failure** - Cannot reserve flight, hotel, or car without payment method. Customer stuck in booking flow, forced to exit and find payment card. Lost time and frustration. |
| **Card Vendor** | Choice | — | Payment processors route transactions differently based on card network (Visa, Mastercard, Amex). Some suppliers don't accept certain card types (e.g., Amex not accepted everywhere). | **Payment failure at checkout** - Customer thinks booking is complete but payment declines due to unsupported card type. Must restart entire booking process. Hotel/flight price may increase during delay. |
| **Name on Card** | Text | 255 chars | Fraud prevention - card name must match traveler or corporate cardholder. Hotels/car rentals verify card at check-in. | **Declined at check-in** - Hotel or car rental desk rejects card if name doesn't match reservation. Customer stranded without accommodation or transportation. Must provide alternate payment on-site. |
| **Card Expiration Date** | Date | YYYY-MM-DD | Payment authorization requires valid card. Expired cards are declined immediately. Future bookings fail if card expires before travel date. | **Payment decline** - Booking fails at checkout if card is expired. For future-dated trips, customer may complete booking but payment fails later when supplier charges card, causing automatic cancellation without warning. |
| **Credit Card Display Name** | Text | 255 chars | Helps customer identify which card is being used (e.g., "Corporate Amex", "Personal Visa"). Prevents accidental use of personal card for business travel. | **Expense violation** - Customer accidentally books with wrong card type, causing expense report rejection and out-of-pocket reimbursement delays. May violate company policy, requiring manager approval or personal payment. |

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

### Contact Information

| Field Name | Type | Max Length | Why Recommended | Impact on Customer Experience |
|------------|------|------------|-----------------|------------------------------|
| **Primary Email Address** | Text (ASCII) | 255 chars | All booking confirmations, itinerary updates, boarding passes, hotel confirmations sent via email. Only reliable way to receive travel documents. | **Lost confirmations and itineraries** - Customer doesn't receive booking confirmation number, boarding pass, hotel address. Must call customer service (30+ min wait) to get confirmation. Risk of missed check-in deadline. Cannot access mobile boarding pass—forced to print at airport. |
| **Primary Mobile Phone Number** | Text | 60 chars | Enables SMS flight alerts (delays, cancellations, gate changes), TextApp receipt submission, hotel check-in notifications, ride-share pickup coordination. | **Missed critical alerts** - Customer unaware of 3-hour flight delay until arrival at airport. Misses gate change, boards wrong flight. Cannot use TextApp to submit receipts (must save paper, manual upload). No SMS confirmation for hotel/car pickup. Adds stress and wasted time. |
| **Phone Country Code** | Choice | 2-letter ISO | Ensures SMS/calls reach correct international phone number format. Prevents messages sent to wrong country code. | **Communication failure** - Airline sends SMS alerts to wrong number (e.g., assumes US +1 instead of UK +44). Customer misses flight delay notification. Hotel cannot reach customer for late check-in issues. |

### Travel Preferences (Pre-fill Booking Requests)

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

### Rate Preferences

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **AAA Rate** | Yes/No | — | Discount rate (10-15%) for AAA members at hotels and car rentals. Requires showing AAA card at check-in. | **Modest savings** - Saves $10-30 per hotel night. Customer must remember to request AAA rate and show card at check-in. If missing from profile, customer can request rate manually during booking. Helpful savings but not mandatory. |
| **AARP Rate** | Yes/No | — | Senior discount (10-20%) for AARP members (50+ years old). Common at hotels and car rentals. | **Senior savings** - Saves $15-40 per night for eligible travelers. Must show AARP card at check-in. If missing, customer requests rate manually. Nice discount but not required for booking. |
| **Government Rate** | Yes/No | — | Special government employee rates at hotels (up to 30% discount). Requires showing government ID at check-in. | **Significant savings** - Government rates can be 30%+ cheaper. Hotels set aside inventory for government travelers. Must verify eligibility at check-in (government ID). If missing, customer can request manually but inventory may be sold out. |
| **Military Rate** | Yes/No | — | Military/veteran discount at hotels, car rentals, airlines (10-25% savings). Requires military ID verification. | **Honor and savings** - Rewards military service with meaningful discounts. Must show military/veteran ID at check-in. If missing from profile, customer can request rate manually. Helpful but not blocking. |

### Rail-Specific Preferences

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Rail Seat** | Choice | — | Aisle vs. window preference for rail travel. Less critical than air travel since trains have more movement freedom. | **Minor comfort** - Nice to have preferred seat but customer can move around on train (unlike plane). If missing, customer selects during booking or accepts assigned seat. Lower impact than flight seating. |
| **Rail Coach Type** | Choice | — | Quiet car, table seating, compartment, or standard coach. Affects comfort for long journeys. | **Comfort preference** - Quiet car nice for work/sleep on long trips. Table seating good for groups. If missing, customer gets standard coach (acceptable for most). Can upgrade on board if available. |
| **Rail Noise Comfort** | Choice | — | Quiet zone vs. mobile phone allowed zones. European trains often have designated quiet carriages. | **Comfort preference** - Quiet zone provides peaceful work environment. Mobile zone allows calls. If missing, customer assigned to standard car (moderate noise). Can move between cars if needed. |
| **BahnCard Type** | Choice | — | Deutsche Bahn discount card (25%, 50%, or 100% off). Significant savings for frequent German rail travelers. | **Major savings for frequent travelers** - BahnCard 50 saves 50% on every German rail ticket. BahnCard 100 = unlimited travel. If missing, customer pays full price (can manually enter discount card at booking). Critical for frequent Germany travelers but niche globally. |
| **BahnCard Class** | Choice | — | First class vs. second class BahnCard. Determines which class receives discount. | **Discount application** - Ensures discount applied to correct class. If missing, discount may apply to wrong class (customer pays more or gets downgraded). Can correct at booking but requires attention. |

### Hotel Amenities

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

### Car Rental Add-Ons

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **GPS** | Yes/No | — | Rental car GPS navigation system. Less relevant in smartphone era (Google Maps, Waze). | **Legacy convenience** - GPS units useful before smartphones. Now most travelers use phone navigation (free, better, real-time traffic). Rental GPS adds $10-15/day (expensive). If missing, customer uses phone—better solution anyway. |
| **Ski Rack** | Yes/No | — | Roof-mounted ski rack for winter sports trips. Niche use case for specific destinations. | **Niche equipment** - Only relevant for ski trips. If missing, customer rents rack at car counter ($20-30) or checks skis at airline. Very specific use case—not relevant for 95%+ of rentals. |
| **Car Smoking Preference** | Yes/No | — | Smoking vs. non-smoking rental car. Most rental fleets now entirely non-smoking due to health regulations. | **Obsolete preference** - Nearly all rental cars non-smoking now (cleaning costs, regulations). Smoking in non-smoking car = $200-500 cleaning fee. If customer smokes, must do so outside vehicle. Low relevance today. |

### Payment Card Defaults

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Default Card for Air** | Yes/No | — | Automatically selects preferred card for flight bookings. Prevents accidental use of wrong card. | **Convenience** - Saves selecting card from dropdown every time. If missing, customer selects card manually (5 seconds). Helpful for frequent travelers with multiple cards but low impact. |
| **Default Card for Car** | Yes/No | — | Pre-selects preferred card for car rentals. | **Convenience** - Auto-fills payment at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Hotel** | Yes/No | — | Pre-selects preferred card for hotel bookings. | **Convenience** - Auto-fills payment at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Rail** | Yes/No | — | Pre-selects preferred card for rail bookings. | **Convenience** - Auto-fills payment at checkout. If missing, customer selects manually. Minor time savings only. |
| **Default Card for Taxi** | Yes/No | — | Pre-selects preferred card for ground transportation and ride-shares. | **Convenience** - Auto-fills payment at checkout. If missing, customer selects manually. Minor time savings only. Ride apps (Uber/Lyft) have own payment methods. |

### Preferences & Settings

| Field Name | Type | Max Length | Why Optional | Impact on Customer Experience |
|------------|------|------------|--------------|------------------------------|
| **Preferred Language** | Choice (BCP 47) | 20 chars | UI display language for Concur interface. Defaults to browser/system language if missing. | **Localization preference** - Customer sees interface in preferred language. If missing, system detects from browser (works for 90% of users). Helpful for multilingual users who want specific language regardless of location. Minor convenience. |
| **Country Code** | Choice | 2-letter ISO | Traveler's home country for default currency, tax calculations, policy assignment. | **System defaults** - Used for currency display, tax forms, regional policy application. If missing, system infers from company location or login IP. Mostly invisible to customer—backend setting. |
| **eReceipt Opt-In** | Yes/No | — | Receive electronic receipts via email instead of paper. Environmental preference and convenience. | **Paperless convenience** - Email receipts easier to organize than paper. Searchable, automatically archived. If missing, customer gets paper receipts (must scan manually for expense reports—extra work). eReceipts significantly faster for expense submission. Should arguably be "Recommended" for productivity.

---

## **Summary by Booking Scenario**

### Domestic Flight (US)
**Required:** Travel CRS Name, First Name, Last Name, Payment Card
**Recommended:** TSA PreCheck KTN, Airline Loyalty Number, Email, Mobile Phone, Seat Preferences
**Optional:** Meal Preference, Special Assistance

### International Flight
**Required:** Travel CRS Name, First Name, Last Name, Middle Name, Passport Number, Passport Expiration, Passport Issuing Country, Nationality, Payment Card
**Recommended:** Visa (if applicable), TSA PreCheck, Airline Loyalty, Email, Mobile Phone
**Optional:** Emergency Contact, Medical Alerts

### Hotel Booking
**Required:** First Name, Last Name, Payment Card
**Recommended:** Hotel Loyalty Number, Email, Mobile Phone, Room Type Preference, Smoking Preference
**Optional:** Amenity Preferences (gym, pool, crib, etc.)

### Car Rental
**Required:** First Name, Last Name, Payment Card, Driver's License Number
**Recommended:** Car Loyalty Number, Email, Mobile Phone, Car Type Preference, Transmission
**Optional:** GPS, Ski Rack

### Rail Booking (Europe)
**Required:** First Name, Last Name, Payment Card
**Recommended:** Rail Loyalty Number, BahnCard (for Deutsche Bahn), Email, Seat Preferences
**Optional:** Coach Type, Meal Preference, Special Assistance

---

## **Validation Rules for Know Me Agent**

### Blocking Validations (Prevent Booking)
- International flight without passport number/expiration
- Any booking without payment card
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
- Credit card expires before travel date

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
✓ Payment Card: ending in 1234, expires 12/2027
✗ Middle Name: Missing (required by airline for international - BLOCK)

Prompt: "I need your middle name to complete this international booking. Airlines require it to match your passport exactly."
```

**Scenario: Fred books domestic US flight**
```
Know Me validates:
✓ Travel CRS Name: FREDERICKS/FRED
✓ Payment Card: ending in 1234
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
✓ All required fields present (passport, payment, legal name)
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
