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

[Copy Corrected Code] [Generate Python Code] [View Documentation]
```

---

## Time to Resolution Comparison

| Approach | Time to Solution | Human Touchpoints | Customer Effort |
|----------|------------------|-------------------|-----------------|
| **Current (Email Support)** | ~1 month (4 weeks) | 6+ emails across 4 people | Very High (multiple test calls, logs, back-and-forth) |
| **With Profile API AI Assistant** | ~3 minutes | 0 (self-service) | Very Low (paste payload, get instant fix) |

**Time saved:** 99.9% reduction (1 month → 3 minutes)

---

## Business Impact

### For Customer (Mott MacDonald):
- **Unblocked immediately:** Developer continues integration work without waiting for support
- **Reduced frustration:** No more multi-week email threads
- **Knowledge retention:** AI explains *why* the fix works, Ben learns for future API calls
- **Cost savings:** 12+ hours of developer time saved ($1,200+ at $100/hr)

### For SAP Concur Support:
- **Ticket deflection:** Ben never opens a support case
- **Reduced escalations:** No need to involve Matt (senior engineer) for basic API syntax issues
- **Support capacity freed:** Ronald/Constantin/Kevin available for complex issues
- **Pattern detection:** AI learns from Ben's error to help future users with similar issues

### Cost Analysis:

**Current State (Email Support):**
- Support engineer time: ~8 hours over 4 weeks (email responses, investigation, escalation)
- Customer developer time: ~12 hours (testing, waiting, retesting)
- **Total cost:** ~$2,000 (20 hours × $100/hr blended rate)

**With Profile API AI Assistant:**
- Support engineer time: 0 hours
- Customer developer time: 3 minutes
- **Total cost:** ~$5 (3 min × $100/hr)

**Savings per case:** $1,995 (99.75% reduction)

---

## Developer Requirements

### What the AI Assistant Must Do:

1. **Analyze API Payload Structure**
   - Parse JSON payload
   - Identify operation type (`add`, `replace`, `remove`)
   - Detect missing required fields based on operation type

2. **Validate SCIM PATCH Syntax**
   - For `replace` operations: Ensure `path` field exists
   - For `add` operations: Ensure `value` contains full object structure
   - Validate SCIM filter syntax in `path` field

3. **Detect Field Name Casing Errors**
   - Profile API uses camelCase: `approvalLimit`, `reimbursementCurrency`
   - Flag PascalCase errors: `ApprovalLimit` → `approvalLimit`

4. **Provide Corrected Payload**
   - Generate syntactically correct version
   - Highlight what changed (diff view)
   - Explain why each change is necessary

5. **Recommend Simpler Alternatives**
   - Detect when `/Bulk` endpoint used for single user
   - Suggest `/User` endpoint as simpler alternative
   - Provide side-by-side comparison

6. **Generate Working Code**
   - Python, Java, C#, JavaScript, etc.
   - Include proper headers, authentication, error handling
   - Add inline comments explaining key parts

7. **Link to Documentation**
   - Surface relevant API docs (User Provisioning, SCIM PATCH)
   - Point to field requirement references
   - Show examples of `add` vs `replace` operations

---

## Key Technical Details for Developer

### SCIM PATCH Operation Rules:

| Operation | Requires `path`? | Use Case | Example |
|-----------|------------------|----------|---------|
| `add` | ❌ No | Creating new attribute | Adding new approver |
| `replace` | ✅ Yes | Updating existing attribute | Changing approval limit |
| `remove` | ✅ Yes | Deleting attribute | Removing approver |

### Common Error Patterns to Detect:

1. **Missing `path` in `replace` operation**
   - Error: `"Request is unparsable, syntactically incorrect, or violates schema"`
   - Fix: Add `path` field with SCIM filter syntax

2. **Incorrect field name casing**
   - Error: Field not recognized or ignored
   - Fix: Change `ApprovalLimit` → `approvalLimit`

3. **Using `/Bulk` for single user updates**
   - Issue: Unnecessary complexity, harder to debug
   - Fix: Recommend `/User` endpoint instead

4. **Invalid approval group**
   - Error: `"Group provided could not be resolved to a valid hierarchy"`
   - Fix: Validate group exists, suggest alternatives

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

## Next Steps for Developer

1. **Implement payload parser** for User Provisioning API requests
2. **Build rule engine** to detect common error patterns
3. **Create fix generator** that produces corrected payloads
4. **Integrate with documentation** to surface relevant links
5. **Add code generator** for Python, Java, C#, JavaScript
6. **Test with real customer payloads** (Ben's example + others from support tickets)
