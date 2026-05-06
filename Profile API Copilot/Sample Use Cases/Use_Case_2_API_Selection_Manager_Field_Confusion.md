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

### Follow-Up: Vishnu Asks for Python Code

```
Vishnu: "Can you generate Python code for this?"

AI Assistant:

Here's working Python code to retrieve users with department, 
cost center, and manager:

```python
import requests

# Configuration
base_url = "https://us.api.concursolutions.com"
access_token = "YOUR_ACCESS_TOKEN"

# Headers
headers = {
    "Authorization": f"Bearer {access_token}",
    "Accept": "application/json"
}

# Specify attributes to retrieve
attributes = [
    "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department",
    "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter",
    "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager"
]

# Build query parameters
params = {
    "attributes": ",".join(attributes)
}

# Optional: Filter by department
# params["filter"] = 'urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department eq "Engineering"'

# Make API request
response = requests.get(
    f"{base_url}/profile/identity/v4/Users",
    headers=headers,
    params=params
)

# Check response
if response.status_code == 200:
    users = response.json()
    
    print(f"Found {len(users.get('Resources', []))} users")
    
    # Process each user
    for user in users.get('Resources', []):
        enterprise_data = user.get('urn:ietf:params:scim:schemas:extension:enterprise:2.0:User', {})
        
        print(f"\nUser: {user.get('userName')}")
        print(f"  Department: {enterprise_data.get('department')}")
        print(f"  Cost Center: {enterprise_data.get('costCenter')}")
        
        manager = enterprise_data.get('manager', {})
        if manager:
            print(f"  Manager: {manager.get('displayName')} (ID: {manager.get('value')})")
        else:
            print(f"  Manager: None")
else:
    print(f"Error: {response.status_code}")
    print(response.text)
```

🔑 Key Points:

1. Replace "YOUR_ACCESS_TOKEN" with your OAuth token
2. Change base_url to your data center (us/eu/cn)
3. The enterprise:2.0:User schema contains the fields you need
4. Manager field returns both ID (value) and name (displayName)

⚠️ Common Errors:

- 401 Unauthorized: Check your access token is valid
- 403 Forbidden: Ensure your token has user.read scope
- 404 Not Found: Verify your data center URL (us vs eu)

[Run in API Playground] [Download Full Script] [View Error Guide]
```
```


| Approach | Time to Solution | Team Escalations | Customer Effort |
|----------|------------------|------------------|-----------------|
| **Current (Slack Support)** | Multiple hours to days | 2+ teams (Core Profile → Travel Profile) | High (multiple clarifications, waiting) |
| **With Profile API AI Assistant** | ~2 minutes | 0 (self-service) | Very Low (ask question, get working code) |

**Time saved:** 99% reduction (hours/days → 2 minutes)

---

## Business Impact

### Problem Solved:
1. **API Selection Confusion** - Customer immediately knows to use Identity v4 API, not Travel Profile or Spend User
2. **Field Ambiguity** - AI clarifies that "manager" in customer's question = "direct reporting manager" = `enterprise:2.0:User:manager`
3. **Team Escalations Avoided** - No need to escalate from Core Profile → Travel Profile → back to Core Profile
4. **Working Code Provided** - Customer gets copy-paste ready Python code

### For Customer:
- ✅ Immediate answer (2 minutes vs hours/days)
- ✅ No confusion about which "manager" field to use
- ✅ Working API call and code provided
- ✅ Understands why NOT to use Travel Profile API

### For SAP Concur Support:
- ✅ No Slack thread escalations
- ✅ No cross-team redirects (Core Profile ↔ Travel Profile)
- ✅ Support engineer capacity freed for complex issues
- ✅ Reduction in "which API should I use?" questions

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
