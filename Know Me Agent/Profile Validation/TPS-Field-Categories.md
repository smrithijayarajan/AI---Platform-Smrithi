# Travel Profile Service (TPS) Field Categories

Based on Travel Profile Service field details, this document categorizes profile fields by their importance for booking scenarios.

---

## **REQUIRED Fields for Booking**

### Identity & Legal Name
- **Travel CRS Name** (Text, 60 chars) - Exact name in GDS, must match ID/passport. Format: LASTNAME/FIRSTNAME
- **First Name** (Text, 60 chars)
- **Last Name** (Text, 60 chars)

### International Travel - REQUIRED
- **Passport Number** (Text, 100 chars)
- **Passport Issuing Country** (Choice, 2-letter ISO)
- **Passport Nationality** (Choice, 2-letter ISO)
- **Passport Expiration Date** (Date, YYYY-MM-DD)
- **Middle Name** (Text, 60 chars) - Required by some airlines for international flights

### Payment
- **Credit Card Account Number** (Text, 150 chars, ASCII only)
- **Card Vendor** (Choice - Visa, Mastercard, Amex, etc.)
- **Name on Card** (Text, 255 chars)
- **Card Expiration Date** (Date, YYYY-MM-DD)
- **Credit Card Display Name** (Text, 255 chars)

---

## **RECOMMENDED Fields for Enhanced Experience**

### TSA/Trusted Traveler (US Travel)
- **TSA PreCheck Number / Known Traveler Number (KTN)** (Text, 150 chars) - Faster security screening
- **Redress Number** (Text, 150 chars) - For travelers on watchlists
- **TSA Date of Birth** (Date, YYYY-MM-DD)
- **TSA Gender** (Choice: Male, Female)

### Loyalty Programs (Earn Points/Miles)
- **Air Loyalty Program Account Number** (Text, 60 chars) - Frequent flyer number
- **Air Loyalty Vendor Code** (Text, 2 chars, ASCII) - e.g., UA, AA, LH
- **Hotel Loyalty Account Number** (Text, 60 chars) - e.g., Marriott Bonvoy, Hilton Honors
- **Car Rental Loyalty Account Number** (Text, 60 chars) - e.g., Hertz Gold
- **Rail Loyalty Account Number** (Text, 60 chars)

### Contact Information
- **Primary Email Address** (Text, 255 chars, ASCII) - For confirmations
- **Primary Mobile Phone Number** (Text, 60 chars) - For SMS alerts, TextApp
- **Phone Country Code** (Choice, 2-letter ISO)

### Travel Preferences (Pre-fill Booking Requests)
- **Home Airport** (Text, 3 chars, IATA code) - Preferred departure airport
- **Air Seat Position (Row)** (Choice: Aisle, Window, Middle, Center, AisleAcross, Isolated, NOPREF)
- **Air Seat Position (Section)** (Choice: Forward, Rear, RearTail, Tail, Bulkhead, Exit, Wing, NOPREF)
- **Air Meal Preference** (Choice - Vegetarian, Kosher, Muslim, GlutenFree, etc.)
- **Hotel Room Type** (Choice: Single, Double, Twin, Queen, King, Disability, DontCare)
- **Hotel Smoking Preference** (Choice: NonSmoking, Smoking, DontCare)
- **Car Type Preference** (Choice - Compact, Midsize, Fullsize, SUV, etc.)
- **Car Transmission** (Choice: Automatic, Manual, DontCare)

### International Travel - RECOMMENDED
- **Visa Number** (Text, 100 chars) - For countries requiring visas
- **Visa Type** (Choice: SingleEntry, DoubleEntry, MultiEntry, ESTA, ETA, SchengenVisa, Unknown)
- **Visa Expiration Date** (Date, YYYY-MM-DD)
- **Visa Country Issued** (Choice, 2-letter ISO)

### Emergency & Safety
- **Emergency Contact Name** (Text, 255 chars)
- **Emergency Contact Phone** (List of phone numbers)
- **Emergency Contact Relationship** (Choice: BRO, SIS, SPS, PRT, PAR, OTH)
- **Medical Alerts** (Text) - *Note: Only first 3 characters preserved in backend sync - effectively limited utility*

---

## **OPTIONAL Fields (Nice-to-Have)**

### Personal Information
- **Name Prefix** (Text, 60 chars) - Mr., Mrs., Dr., Prof.
- **Middle Name** (Text, 60 chars) - Unless required for international
- **Name Suffix** (Text, 60 chars) - Jr., Sr., III
- **Preferred Name** (Text, 60 chars) - Display only, not on tickets
- **Date of Birth** (Date, YYYY-MM-DD)
- **Gender** (Choice: Male, Female, Unspecified, Undisclosed, Unknown)

### Work/Organization
- **Job Title** (Text, 255 chars)
- **Company Employee ID** (Text, 48 chars)
- **Cost Center** (Text, 25 chars)
- **Rule Class** (Text, 60 chars) - Travel policy tier

### Additional Contact
- **Home Address** (Street, City, State, Country, Zip)
- **Work Address** (Street, City, State, Country, Zip)
- **Secondary Email Addresses** (work2, other2)
- **Additional Phone Numbers** (work, home, fax, pager)

### Additional Travel Documents
- **Driver's License Number** (Text, 200 chars)
- **Driver's License Issuing Country/State** (Choice, 2-letter ISO / Text, 30 chars)
- **Driver's License Expiration Date** (Date, YYYY-MM-DD)
- **National ID Document Number** (Text, 200 chars sync limit)

### Rate Preferences
- **AAA Rate** (Yes/No) - AAA member discounts
- **AARP Rate** (Yes/No) - Senior discounts
- **Government Rate** (Yes/No) - Government employee rates
- **Military Rate** (Yes/No) - Military/veteran rates

### Rail-Specific Preferences
- **Rail Seat** (Choice: Aisle, Window, DontCare)
- **Rail Coach Type** (Choice: Coach, CoachWithTable, Compartment, DontCare)
- **Rail Noise Comfort** (Choice: QuietSpace, MobileSpace, DontCare)
- **BahnCard Type** (Choice: Card25, Card50, Card100, Business25, Business50, NA) - Deutsche Bahn only
- **BahnCard Class** (Choice: First, Second, NA)

### Hotel Amenities
- **Prefer Foam Pillows** (Yes/No)
- **Prefer Crib** (Yes/No)
- **Prefer Rollaway Bed** (Yes/No)
- **Prefer Gym** (Yes/No)
- **Prefer Pool** (Yes/No)
- **Prefer Restaurant** (Yes/No)
- **Prefer Room Service** (Yes/No)
- **Prefer Early Check-In** (Yes/No)
- **Prefer Wheelchair Access** (Yes/No)
- **Prefer Access for Blind** (Yes/No)

### Car Rental Add-Ons
- **GPS** (Yes/No)
- **Ski Rack** (Yes/No)
- **Car Smoking Preference** (Choice: NonSmoking, Smoking, DontCare)

### Payment Card Defaults
- **Default Card for Air** (Yes/No)
- **Default Card for Car** (Yes/No)
- **Default Card for Hotel** (Yes/No)
- **Default Card for Rail** (Yes/No)
- **Default Card for Taxi** (Yes/No)

### Preferences & Settings
- **Preferred Language** (Choice, BCP 47 format) - e.g., en-US, de-DE, fr-FR
- **Country Code** (Choice, 2-letter ISO) - Home country
- **eReceipt Opt-In** (Yes/No) - Electronic receipts

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

### Warning Validations (Allow Booking, Show Warning)
- Passport expires within 6 months of travel date (many countries require 6-month validity)
- Visa expiration before return date
- Driver's license expired for car rental
- Credit card expires before travel date

### Recommended Validations (Suggest but Don't Block)
- Missing TSA PreCheck for US domestic flights (suggest adding to save time)
- Missing airline loyalty number (suggest adding to earn miles)
- Missing hotel loyalty number (suggest adding to earn points)
- Missing mobile number (suggest adding for SMS alerts and TextApp)
- Missing email (suggest adding for booking confirmations)

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

Prompt: "You're not enrolled in TSA PreCheck. Adding your Known Traveler Number can save you 15+ minutes at security. Want to add it now, or skip?"
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
