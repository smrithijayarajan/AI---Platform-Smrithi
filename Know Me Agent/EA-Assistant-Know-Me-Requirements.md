# EA Assistant Know Me Requirements

## JIRA Ticket: CP-37633

**Title:** Build Profile "Know Me" Agent for Executive Assistant Co-Pilot Integration

---

## Summary

Build the Profile "Know Me" Agent to provide centralized user profile data, behavioral intelligence, and preference management capabilities for the Executive Assistant Co-Pilot. The agent will enable arrangers and assistants to access, validate, and update traveler preferences and profile information through a unified API, supporting personalized booking experiences and proactive validation of missing or expired traveler data.

---

## Background

From the integration meeting on April 15, the team identified requirements for how the Executive Assistant co-pilot should access and manage traveler preferences. Key discussion points:

1. **Preference Data Access**: The assistant needs to read traveler preferences (flight, hotel, loyalty programs) and potentially learn from user conversations
2. **Validation Use Cases**: The assistant should validate missing/expired information (passport, visa, TSA PreCheck) before booking and surface actionable work items to arrangers
3. **Arranger Permissions**: Arrangers can read and update a defined set of profile fields for all users they manage
4. **Integration Architecture**: Starting with read-only API access, with future extension to agent-to-agent communication for updates

---

## Business Value

- **Reduce booking friction**: Proactively identify missing profile data before booking failures occur
- **Enable arranger efficiency**: Surface actionable work items (missing passport, expired visa) in assistant dashboard
- **Personalized experiences**: Provide behavioral intelligence (preferences, patterns) to enable intelligent recommendations
- **Compliance**: Ensure travelers have required documentation before booking (especially international travel)
- **Single source of truth**: Centralize profile data access across all agents (Booking, Expense, Approval, Assistant)

---

## User Stories

### 1. Read Traveler Preferences (Arranger Use Case)

**As an** arranger using the Executive Assistant co-pilot
**I want to** view a traveler's preferences (flight class, seat, hotel brand, loyalty programs)
**So that** I can book trips that match their preferences without asking redundant questions

#### Acceptance Criteria:
- Know Me API returns traveler preferences scoped to booking use case
- Includes: flight preferences (class, seat), hotel preferences (brand, room type), loyalty programs, ground transport
- Arranger can access preferences for all users they manage

---

### 2. Validate Missing Profile Information (Proactive Validation)

**As an** Executive Assistant co-pilot
**I want to** validate that a traveler has required profile information before booking
**So that** I can prevent booking failures and prompt arrangers to complete missing fields

#### Acceptance Criteria:
- Know Me API returns profile completeness status for booking context
- Validates required fields based on trip type (domestic vs. international)
- For international trips: passport number, passport expiration, visa information
- For domestic trips: TSA PreCheck (recommended), loyalty programs (optional)
- Returns missing/expired fields with actionable recommendations

#### Example Response:
```
Profile Validation:
✓ All required fields present for domestic booking
✓ TSA PreCheck enrolled
⚠ International booking requires: Passport expiration date
✗ Passport expires in 3 months (may not meet destination requirements)
```

---

### 3. Generate Work Items for Arrangers (EA Co-Pilot Team)

**As an** arranger
**I want to** see actionable work items in my dashboard when travelers have missing/expired profile information
**So that** I can proactively update their profiles before they attempt to book

#### Acceptance Criteria:
- Know Me agent generates work items for missing/expired critical fields
- Work items include: field name, reason (missing/expired), traveler name, priority (critical/recommended)
- Arrangers see work items for all managed travelers
- Work items link directly to profile update UI

#### Example Work Item:
```
⚠ Action Required: Fred Fredericks
Missing: Passport expiration date
Context: International trip to London scheduled Apr 20
Priority: Critical (blocks booking)
Action: [Update Profile]
```

---

### 4. Arranger Updates Profile Fields (Write Access)

**As an** arranger
**I want to** update a traveler's profile fields directly from the assistant interface
**So that** I can complete missing information without navigating to separate systems

#### Acceptance Criteria:
- Arrangers can update defined set of profile fields for managed users
- Updates via Know Me API with audit trail (who updated, when, what changed)
- If arranger cannot update a field, system notifies traveler to take action

---

### 5. Behavioral Intelligence for Personalization (Predictive Preferences)

**As an** Executive Assistant co-pilot
**I want to** access a traveler's behavioral patterns and preferences
**So that** I can make intelligent booking recommendations without asking redundant questions

#### Acceptance Criteria:
- Know Me returns behavioral intelligence based on historical bookings
- Includes: preferred departure airports, typical booking advance window, hotel brand loyalty, meal preferences
- Confidence scores for each pattern (e.g., "Books red-eye flights: 94% confidence, 8 trips")
- Predictive intelligence: next likely trip, expected booking preferences

#### Example Response:
```
Travel Intelligence:
- First Class preference, window seat 4A (97% confidence, 15 trips)
- Marriott Bonvoy Platinum Elite (always first choice, 100% confidence)
- Books 12-15 days in advance
- Solo traveler, prefers 7:00 PM dinner reservations
```

---

### 6. Assistant-Specific Preferences (Future Enhancement)

**As an** Executive Assistant
**I want to** learn and store preferences specific to how I assist a traveler
**So that** I can prioritize work items and communications based on their working style

#### Acceptance Criteria:
- Know Me stores assistant-specific metadata (not user profile data)
- Examples: "Prioritizes urgent travel changes over routine bookings", "Prefers morning communications"
- Assistant can query and update its own learned preferences
- Separate from traveler profile data (different namespace)

---

## Integration Points

### Executive Assistant Co-Pilot Integration
- **Read access**: Arranger queries Know Me for traveler preferences
- **Validation**: Before booking, validate profile completeness
- **Work items**: Dashboard displays missing/expired profile data
- **Updates**: Arranger updates profile fields via assistant interface

### Booking Agent Integration
- **Preference application**: Auto-apply learned preferences (seat, hotel, etc.)
- **Missing field prompts**: Conversational prompts during booking flow
- **Predictive suggestions**: Show recommendations based on behavioral patterns

### Identity Service Integration
- **Profile storage**: Know Me reads/writes to Identity Service
- **Permissions**: Arranger permissions managed via Identity Service
- **Audit trail**: All profile updates logged with user JWT

---

## Security & Permissions

### Arranger Permissions
- **Read access**: All travelers they manage
- **Write access**: Defined set of profile fields (loyalty programs, travel documents)
- **Restricted fields**: Sensitive data (SSN, personal health) requires traveler self-service

### Audit Trail
- All profile updates logged with:
  - Who made the change (arranger or traveler)
  - What changed (field name, old value, new value)
  - When it changed (timestamp)
  - Why it changed (context: "Updated for London trip booking")

### Data Masking
- Sensitive fields masked in API responses (e.g., passport: X1234****)
- Full values only accessible with elevated permissions
- Audit log for all sensitive field access

---

## Success Metrics

- **Booking failure reduction**: % decrease in bookings failing due to missing profile data
- **Arranger efficiency**: Time saved per booking (target: 5-10 minutes)
- **Profile completeness**: % of travelers with 100% required fields for their typical trips
- **Work item completion**: % of arranger work items resolved within 24 hours
- **Preference accuracy**: % of bookings using auto-applied preferences without modification

---

## Implementation Phases

### Phase 1: Read-Only API (MVP)
- Implement GET endpoints for preferences, validation, work items
- Arranger can view traveler preferences and missing fields
- Dashboard displays work items but requires manual profile updates in separate system

### Phase 2: Write Access
- Implement PATCH endpoint for profile updates
- Arranger can update profile fields directly from assistant interface
- Audit trail and permissions enforcement

### Phase 3: Behavioral Intelligence
- Implement pattern learning from historical bookings
- Confidence scores for preferences
- Predictive intelligence (next likely trip, expected preferences)

### Phase 4: Agent-to-Agent Communication
- Know Me agent communicates directly with Booking Agent
- Proactive profile enrichment during booking flow
- Assistant-specific preference learning

---

## Related Documentation

- [Know Me Agent Main Requirements](./Profile-Know-Me-Agent-Requirements.md)
- [Executive Assistant Co-Pilot Architecture](https://confluence.concur.com/display/EACOP)
- JIRA: [CP-37633](https://jira.concur.com/browse/CP-37633)
