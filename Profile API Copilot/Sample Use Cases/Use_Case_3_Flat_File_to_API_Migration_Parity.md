# Use Case: Flat File to API Migration Parity Issues

## Customer Context
**Company:** Tetra Pak  
**Project:** Migration from SAP ECC to SAP S4/HANA with Concur native integration  
**User:** Customer IT Team (via Gabriela and Hendrik - SAP Consultants)  
**Problem:** Migrating from flat file imports to UPS v4 API, encountering parity issues between old and new methods

---

## The Problem (Current State - Without AI Assistant)

### Customer's Migration Scenario:
```
"Tetra Pak is migrating from SAP ECC to SAP S4H and uses SAP ICS 
(native integration) for user provisioning. They maintain HR master 
data in custom S4 tables and need to heavily customize the Employee 
provisioning interface using UPS v4 bulk endpoints."
```

### Specific Issue (Point 9):
```
"For Flat file imports (TP's current solution from SAP ECC), there 
is a settings in HMC that disables the option to overwrite Name 
fields by imports. This is NOT applicable on UPS v4 unfortunately, 
and from S4, TP keeps overwriting manual changes to Name fields 
which is blocking TP from go-live later this year."
```

### The Confusion:
- **Flat file import behavior:** HMC setting allows protecting Name fields from being overwritten
- **API behavior:** No equivalent setting exists for UPS v4 API
- **Result:** API imports from S4 overwrite manual Name field changes (e.g., passport names, preferred names)

### Current Resolution Process:
1. Customer reports issue via consultant
2. Consultant creates detailed issue list (8+ open points)
3. Escalation to Concur Product/Dev for statement
4. Customer proposes workarounds but doesn't know if feasible:
   - Can Name fields be made non-mandatory?
   - Would blank values prevent overwrites?
   - Can custom fields (Passport First Name, Passport Surname) sync to GDS instead?
5. SAP ICS constraint: Cannot remove schemas from payload (bulk endpoints), only send blank values
6. Sending blank values triggers errors due to mandatory field validation

**Blocking impact:** Delays SIT (System Integration Testing) and go-live

---

## The Solution (New State - With Profile API AI Assistant)

### Scenario: Consultant Uses Profile API AI Assistant

**Consultant opens Profile API AI Assistant:**

```
Consultant: "My customer is migrating from flat file imports to 
            UPS v4 API. In flat file, they have an HMC setting 
            that prevents Name fields from being overwritten. 
            How do we achieve the same with the API?"

AI Assistant:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔍 PARITY GAP IDENTIFIED

Flat File Import vs. UPS v4 API

Feature: Protect Name fields from overwrites
├─ Flat File: ✅ HMC setting "Disable overwrite Name fields"
└─ UPS v4 API: ❌ No equivalent setting (parity gap)

This is a known limitation. The API does not respect the HMC setting 
that protects Name fields during flat file imports.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ RECOMMENDED SOLUTION: Use Field-Level Control

Instead of importing Name fields via API, use a hybrid approach:

Option 1: Exclude Name Fields from API Payload (Preferred)
- Use User Provisioning API (not Bulk) for individual user updates
- Omit Name fields from the PATCH operation payload
- Only update fields that should sync from S4 (department, cost center, etc.)

Example PATCH request (excludes Name fields):
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations": [
    {
      "op": "replace",
      "value": {
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
          "department": "Engineering",
          "costCenter": "CC-ENG-001"
        }
      }
    }
  ]
}

⚠️ Your Constraint: SAP ICS uses Bulk endpoints
Since you're using SAP ICS with bulk endpoints, you CANNOT remove 
schemas from the payload. Let me show you Option 2.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ OPTION 2: Send Existing Values (Not Blank)

Problem with sending blank values:
- Blank Name fields trigger mandatory field errors
- Empty strings are rejected by validation

✅ Solution: Read-before-write pattern
1. GET current user data (including current Name fields)
2. PATCH with S4 data EXCEPT Name fields (use existing Name values)

Example workflow:
Step 1: GET existing user
GET /profile/identity/v4/Users/{userId}

Response includes:
{
  "name": {
    "givenName": "John",      ← Keep this
    "familyName": "Doe"       ← Keep this
  },
  "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
    "department": "Old Department",  ← Update this from S4
    "costCenter": "Old CC"           ← Update this from S4
  }
}

Step 2: PATCH with S4 data + preserved Name fields
{
  "Operations": [
    {
      "op": "replace",
      "value": {
        "name": {
          "givenName": "John",        ← Preserved from Step 1
          "familyName": "Doe"         ← Preserved from Step 1
        },
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
          "department": "New Dept",   ← Updated from S4
          "costCenter": "New CC"      ← Updated from S4
        }
      }
    }
  ]
}

Result: Name fields remain unchanged, other fields updated from S4

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ OPTION 3: Custom Fields for Passport Names

Your suggested workaround is feasible with configuration:

1. Create Custom Fields:
   - Custom Field: "Passport First Name" (maintained by user)
   - Custom Field: "Passport Last Name" (maintained by user)

2. Configure GDS Profile Sync:
   ⚠️ Limitation: Standard GDS sync uses core Name fields (givenName, familyName)
   
   Custom fields CAN be synced to GDS if:
   - You use Travel Profile API to map custom fields to GDS
   - Your GDS supports custom field mapping (check with GDS provider)
   
   This requires custom integration work beyond standard SAP ICS.

3. Use Cases:
   ✅ User manually maintains passport name in custom fields
   ✅ S4 updates core Name fields (won't affect GDS if custom sync configured)
   ✅ GDS booking uses passport name from custom fields

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ ADDRESSING YOUR WORKAROUND QUESTIONS

Q: "Can Name fields be made non-mandatory and would sending blank 
    values stop overwrites?"

A: ❌ No, this is not feasible:
   - Name fields (givenName, familyName) are mandatory in SCIM standard
   - Concur enforces this at API level
   - Blank values will be rejected with 400 Bad Request
   - Making them non-mandatory would break GDS integrations

Q: "Can custom fields sync with GDS instead of standard Name fields?"

A: ⚠️ Partially feasible but requires custom work:
   - Standard SAP ICS does NOT support this
   - Requires custom Travel Profile API integration
   - Depends on GDS provider supporting custom field mapping
   - Not a plug-and-play solution

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 RECOMMENDED APPROACH FOR TETRA PAK

Given your constraints (SAP ICS bulk endpoints, S4 integration):

Phase 1: Implement Read-Before-Write Pattern (Option 2)
✓ Works with SAP ICS bulk endpoints
✓ No Concur product changes needed
✓ Preserves manual Name field changes
✓ Can be implemented immediately

Implementation:
1. Modify SAP ICS integration to GET existing user before PATCH
2. Preserve Name fields from GET response
3. Merge S4 data with preserved Name fields
4. Send complete payload to bulk endpoint

Phase 2 (Optional): Custom Field Solution (Option 3)
- If passport names differ from HR names
- Requires custom Travel Profile API integration
- Evaluate based on business requirement priority

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 PRODUCT GAP ACKNOWLEDGMENT

Yes, this is a parity gap between flat file and API:
- Flat File: HMC setting protects Name fields
- API: No equivalent field-level protection

Feature Request Status:
This has been requested by multiple customers. The read-before-write 
pattern is the current recommended approach until/if field-level 
protection is added to the API.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

