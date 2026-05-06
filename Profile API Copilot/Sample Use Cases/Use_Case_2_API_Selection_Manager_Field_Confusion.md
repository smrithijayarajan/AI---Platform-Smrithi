# Use Case: API Selection for User Data Retrieval

## Customer Context
**User:** Vishnu T R (Customer Developer)  
**Problem:** Confusion about which API to use for retrieving user profile data (department, cost center, manager)  
**Channel:** Slack support thread escalated between teams

---

## The Problem (Current State - Without AI Assistant)

### Customer's Initial Question:
```
"We have a requirement to identify users based on department, 
cost center, manager. Could you please let us know if these 
data is available with profile team. Also if you could please 
let us know if there is any APIS available to fetch this."
```

### The Confusion:
The support engineer (Samuel Jansen) immediately asks for clarification:
```
"What manager do you mean? 
- biManager
- cost center approver
- workflow approver  
- travel manager
- hrManager?"
```

**Why This Confusion Exists:**
- The Profile API ecosystem has multiple manager-related fields across different schemas
- "Manager" is ambiguous - could refer to 5+ different fields
- Customer doesn't know which API endpoint to use (Identity, Spend User, Travel Profile, Orchestrated Get)
- Core Profile team redirected to Travel Profile team, creating escalation

### Customer's Clarification:
```
"Manager as I meant direct reporting manager. Along with 
cost center and department.

We initially reached out to core profile and they suggested 
this data is from travel-profile."
```

### Current Resolution Process:
1. Customer asks question in Slack
2. Support engineer asks for clarification on "manager" field
3. Customer clarifies "direct reporting manager"
4. Core Profile team redirects to Travel Profile team
5. Customer still doesn't know the exact API endpoint or field names
6. Likely requires additional back-and-forth to get working API call

**Time to resolution:** Multiple hours/days with team escalations

---

## The Solution (New State - With Profile API AI Assistant)

### Scenario: Vishnu Uses Profile API AI Assistant

**Customer opens Profile API AI Assistant:**

```
Vishnu: "I need to identify users based on department, cost center, 
        and manager. Which API should I use?"

AI Assistant:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ RECOMMENDATION: Use Identity v4 API

For retrieving user profile data including department, cost center, 
and direct reporting manager, use the Identity v4 API with the 
GET /Users endpoint.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 API ENDPOINT

GET https://us.api.concursolutions.com/profile/identity/v4/Users

Query Parameters:
- filter: Filter users by specific criteria
- attributes: Specify which fields to return

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔍 FIELD MAPPING

Your requirement → API field:

1. Department → "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department"
2. Cost Center → "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter"  
3. Manager (direct reporting) → "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager"

⚠️ Note: "Manager" can mean different things in SAP Concur:
- Direct Reporting Manager → enterprise:2.0:User:manager ✓ (This is what you need)
- Expense Approver → spend:2.0:Approver
- Travel Approver → travel:2.0:User:manager
- Workflow Approver → spend:2.0:WorkflowPreference
- Cost Center Approver → spend:2.0:ApproverLimit

You specified "direct reporting manager" so I'm using enterprise:2.0:User:manager

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💻 WORKING EXAMPLE

GET https://us.api.concursolutions.com/profile/identity/v4/Users?attributes=urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department,urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter,urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager

Headers:
  Authorization: Bearer {access_token}
  Accept: application/json

Response Example:
{
  "Resources": [
    {
      "id": "12345-67890-abcde",
      "userName": "john.doe@company.com",
      "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
        "department": "Engineering",
        "costCenter": "CC-ENG-001",
        "manager": {
          "value": "98765-43210-fghij",
          "displayName": "Jane Smith"
        }
      }
    }
  ]
}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 FILTERING USERS

If you want to filter by department or cost center:

Example: Get all users in "Engineering" department
GET /Users?filter=urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department eq "Engineering"

Example: Get all users in cost center "CC-ENG-001"  
GET /Users?filter=urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter eq "CC-ENG-001"

Example: Get all users reporting to a specific manager
GET /Users?filter=urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager.value eq "98765-43210-fghij"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 ALTERNATIVE APIs (Why Not to Use Them)

❌ Travel Profile API
- Contains travel-specific data (seat preferences, TSA number)
- Does NOT contain department or cost center
- Contains travel manager (different from direct reporting manager)

❌ Spend User API  
- Contains expense-specific data (reimbursement method, delegates)
- Manager field here refers to expense approver, not direct reporting manager

✅ Identity v4 API (Recommended)
- Single source of truth for user identity data
- Contains department, cost center, direct reporting manager
- Uses standard SCIM schema
- Supports filtering and pagination

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Copy API Call] [Generate Code (Python/Java/C#)] [View Full Documentation]

Need help with authentication or have more questions?
```

---



## Key Learning for AI Assistant

### Pattern: "Manager" Ambiguity

When customer asks for "manager" field, AI must:

1. **Clarify which type of manager:**
   - Direct Reporting Manager (enterprise:2.0:User:manager)
   - Expense Approver (spend:2.0:Approver)
   - Travel Manager (travel:2.0:User:manager)
   - Workflow Approver (spend:2.0:WorkflowPreference)
   - Cost Center Approver (spend:2.0:ApproverLimit)

2. **Recommend correct API based on context:**
   - "department + cost center + manager" → Identity v4 API
   - "expense approver + reimbursement" → Spend User API
   - "travel preferences + seat" → Travel Profile API

3. **Explain why NOT to use wrong API:**
   - Travel Profile API does NOT have department or cost center
   - Spend User API's "manager" is expense approver, not reporting manager

### Pattern: Field Name Translation

Customer language → API field mapping:
- "manager" → `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager`
- "department" → `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department`
- "cost center" → `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter`

AI should translate natural language to exact SCIM schema paths automatically.
