# Use Case: User Provisioning API Error Resolution

## Customer Context
**Company:** Mott MacDonald (Large Engineering Consultancy)  
**User:** Ben Staples, IT Developer - Data Integration  
**Problem:** Struggling with User Provisioning API errors for weeks, requiring multiple back-and-forth emails with SAP support team

---

## The Problem (Current State - Without AI Assistant)

### Timeline of Frustration:
- **Sept 22, 2025:** Ben sends API error to support, attaches 4 documents
- **Oct 6, 2025:** Still troubleshooting, sends new provisioning ID with payload
- **Oct 17, 2025:** Support asks for another fresh request
- **Oct 21, 2025:** Ben sends yet another test with provisioning ID
- **Oct 23, 2025:** Matt (former support engineer) provides solution after being escalated to

**Total time to resolution:** ~1 month of back-and-forth emails

### The Specific Error:
```json
{
  "code": "BAD_REQUEST",
  "message": "Request is unparsable, syntactically incorrect, or violates schema.",
  "schemaPath": "urn:ietf:params:scim:schemas:extension:spend:2.0:ApproverLimit:costObjectApprover[approvalType eq \"report\" and approvalGroup eq \"100108234\"]"
}
```

### Ben's Failing API Payload:
```json
{
  "method": "PATCH",
  "path": "/Users/f10dbb63-8f4f-4cd5-b640-8ecd66c14fa4",
  "data": {
    "Operations": [
      {
        "value": {
          "urn:ietf:params:scim:schemas:extension:spend:2.0:ApproverLimit": {
            "costObjectApprover": [
              {
                "exceptionApprovalAuthority": false,
                "approvalType": "report",
                "ApprovalLimit": 10000000.00,
                "ReimbursementCurrency": "GBP",
                "ApprovalGroup": "100108234"
              }
            ]
          }
        },
        "op": "replace"  ❌ Missing 'path' field
      }
    ]
  }
}
```

### Root Cause (Eventually Discovered):
The `Operations` body was missing a required `path` field when using the `replace` operation for updating ApproverLimit in SCIM PATCH requests.

---

## The Solution (New State - With Profile API AI Assistant)

### Scenario: Ben Uses Profile API AI Assistant

**Oct 21, 2025, 12:26 PM - Ben opens Profile API AI Assistant:**

```
Ben: "I'm getting a BAD_REQUEST error when trying to update a user's 
      ApproverLimit via the User Provisioning API. Here's my request:"

[Pastes API payload]

AI Assistant: 
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 ERROR IDENTIFIED

Your 'replace' operation is missing the required 'path' field.

Current payload (incorrect):
{
  "Operations": [
    {
      "value": { ... },
      "op": "replace"  ❌ Missing 'path' field
    }
  ]
}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ CORRECTED PAYLOAD

{
  "method": "PATCH",
  "path": "/Users/f10dbb63-8f4f-4cd5-b640-8ecd66c14fa4",
  "data": {
    "Operations": [
      {
        "op": "replace",
        "path": "urn:ietf:params:scim:schemas:extension:spend:2.0:ApproverLimit.costObjectApprover[approvalType eq \"report\" and approvalGroup eq \"100108234\"]",
        "value": {
          "approvalLimit": 10000000.00,
          "reimbursementCurrency": "GBP",
          "exceptionApprovalAuthority": false
        }
      }
    ]
  }
}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 WHAT CHANGED

1. Added 'path' field targeting specific costObjectApprover entry
2. Used SCIM filter syntax: approvalType eq "report" and approvalGroup eq "100108234"
3. Fixed field name casing: ApprovalLimit → approvalLimit
4. Fixed field name casing: ReimbursementCurrency → reimbursementCurrency
5. Fixed field name casing: ApprovalGroup → approvalGroup

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💡 RECOMMENDATION: Use /User Endpoint Instead

Since you're updating a single user, the /User endpoint is simpler 
than /Bulk and easier to debug:

POST https://eu2.api.concursolutions.com/profile/v4/Users/f10dbb63-8f4f-4cd5-b640-8ecd66c14fa4

{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations": [
    {
      "op": "replace",
      "path": "urn:ietf:params:scim:schemas:extension:spend:2.0:ApproverLimit.costObjectApprover[approvalType eq \"report\" and approvalGroup eq \"100108234\"]",
      "value": {
        "approvalLimit": 10000000.00,
        "reimbursementCurrency": "GBP",
        "exceptionApprovalAuthority": false
      }
    }
  ]
}



5. **Missing spend profile**
   - Error: `"Extension is required for provisioning a Spend User"`
   - Fix: Guide user to create spend profile first

---

## Success Criteria

### Phase 1 (Internal Consultants):
- ✅ Detect missing `path` field in `replace` operations
- ✅ Correct field name casing errors
- ✅ Recommend `/User` endpoint when `/Bulk` used for single user
- ✅ Generate working code in Python/Java
- ✅ Link to relevant documentation

### Phase 2 (Customers & Partners):
- ✅ Reduce API-related support tickets by 80%
- ✅ Achieve 3-minute average resolution time for common errors
- ✅ Deflect $60,000/month in support costs ($720,000/year)

---

## Example AI Response Template (For Developer)

```python
# Pseudo-code for AI Assistant logic

def analyze_user_provisioning_payload(payload):
    """
    Analyzes User Provisioning API payload and identifies errors
    """
    errors = []
    fixes = []
    
    # Extract operation details
    operations = payload.get("data", {}).get("Operations", [])
    
    for op in operations:
        operation_type = op.get("op")
        
        # Rule 1: Replace operations must have 'path' field
        if operation_type == "replace" and "path" not in op:
            errors.append({
                "type": "missing_path",
                "message": "Replace operation missing required 'path' field",
                "severity": "critical"
            })
            
            # Generate fix
            if "value" in op:
                # Infer path from value structure
                schema_path = infer_scim_path(op["value"])
                fixes.append({
                    "add_field": "path",
                    "suggested_value": schema_path
                })
        
        # Rule 2: Check field name casing
        if "value" in op:
            casing_errors = check_field_casing(op["value"])
            if casing_errors:
                errors.append({
                    "type": "incorrect_casing",
                    "fields": casing_errors
                })
                fixes.append({
                    "fix_casing": casing_errors
                })
    
    # Rule 3: Recommend /User endpoint if single user
    if len(operations) == 1 and "/Bulk" in payload.get("endpoint", ""):
        errors.append({
            "type": "suboptimal_endpoint",
            "message": "Using /Bulk for single user update",
            "severity": "warning"
        })
        fixes.append({
            "recommendation": "Use /User endpoint instead of /Bulk"
        })
    
    return {
        "errors": errors,
        "fixes": fixes,
        "corrected_payload": generate_corrected_payload(payload, fixes)
    }

def infer_scim_path(value_object):
    """
    Infers SCIM path from value object structure
    Example: Converts nested ApproverLimit object to proper SCIM filter path
    """
    # Implementation details...
    pass

def check_field_casing(obj):
    """
    Checks if field names use correct camelCase
    Returns list of fields with incorrect casing
    """
    incorrect_fields = []
    correct_mappings = {
        "ApprovalLimit": "approvalLimit",
        "ReimbursementCurrency": "reimbursementCurrency",
        "ApprovalGroup": "approvalGroup",
        # ... more field mappings
    }
    
    for key in obj.keys():
        if key in correct_mappings:
            incorrect_fields.append({
                "incorrect": key,
                "correct": correct_mappings[key]
            })
    
    return incorrect_fields
```

---

