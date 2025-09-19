# GitHub Copilot Security Instructions - AVAST Framework

## Security-First Code Generation

You MUST follow the AVAST security framework for all code:

**A - Authentication**: 
- Use bcrypt/argon2 only (min 10 rounds)
- Parameterized queries mandatory
- Session regeneration required

**V - Validation**:
- Validate all inputs
- Sanitize all outputs  
- No eval/innerHTML with user data

**A - API Security**:
- Rate limiting required
- Authentication on all endpoints
- Minimal data exposure

**S - Secrets**:
- Environment variables only
- No hardcoded credentials
- No secrets in logs

**T - Trust**:
- Zero trust on user input
- Verify all permissions
- Server-side validation only

## Blocked Patterns:
```javascript
// NEVER GENERATE:
password === userPassword
MD5(password)
`SELECT * WHERE id = ${id}`
eval(userInput)
innerHTML = userContent
apiKey = "sk-abc123"
```
## Required Patterns:
javascript
```
ALWAYS GENERATE:
bcrypt.compare(password, hash)
bcrypt.hash(password, 12)
query('SELECT * WHERE id = ?', [id])
DOMPurify.sanitize(input)
textContent = userContent
process.env.API_KEY
```
### **For Python Projects (`.ai-rules.md`)**
# AI Code Generation Security Rules - Python

## AVAST Compliance Required

### Authentication Security
```python
# REQUIRED:
from argon2 import PasswordHasher
ph = PasswordHasher()
hashed = ph.hash(password)
```
# FORBIDDEN:
import hashlib
hashed = hashlib.md5(password.encode()).hexdigest()
Validation Security
python# REQUIRED:
from sqlite3 import connect
conn.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# FORBIDDEN:
query = f"SELECT * FROM users WHERE id = {user_id}"
API Security
python# REQUIRED:
from flask_limiter import Limiter
@limiter.limit("5 per minute")
@requires_auth
def api_endpoint():
    return jsonify({"data": filtered_data})

# FORBIDDEN:
@app.route('/api/internal')
def api_endpoint():
    return jsonify(User.query.all())
Secrets Management
python# REQUIRED:
import os
API_KEY = os.environ.get('API_KEY')
if not API_KEY:
    raise ValueError("API_KEY not configured")

# FORBIDDEN:
API_KEY = "sk-1234567890abcdef"
Trust Boundaries
python# REQUIRED:
from bleach import clean
safe_html = clean(user_input)

# FORBIDDEN:
return f"<div>{user_input}</div>"
Generate with security context:

Ask about threat model before generating
Include security comments in code
Add validation for edge cases
Implement defense in depth


### **For Infrastructure/Terraform (`.tfsec-ai.md`)**
# Terraform AI Generation Rules - AVAST

## Infrastructure Security Requirements

### Authentication
- Enable MFA on all IAM roles
- Use temporary credentials only
- Implement least privilege

### Validation
- Validate all input variables
- Use data sources for verification
- Implement policy as code

### API Exposure
- Private subnets by default
- API Gateway with authentication
- WAF rules required

### Secrets
- Use AWS Secrets Manager/Vault
- Never hardcode credentials
- Rotate keys automatically

### Trust
- Zero trust network architecture
- Verify all peer connections
- Audit logging enabled

## Example:
```hcl
# ALWAYS:
variable "api_key" {
  type        = string
  sensitive   = true
  description = "API key from environment"
}

resource "aws_secretsmanager_secret" "api_key" {
  name = "api-key"
}

# NEVER:
variable "api_key" {
  default = "sk-abc123..."
}
```
### **Universal Addition for Any File**
## 🚨 AVAST SECURITY CHECK 🚨
Before accepting ANY AI-generated code, verify:
- [ ] A: No hardcoded passwords, SQL concatenation, weak hashing
- [ ] V: All inputs validated, outputs sanitized
- [ ] A: APIs authenticated, rate-limited, minimal exposure  
- [ ] S: No secrets in code, use env vars
- [ ] T: Zero trust on inputs, verify permissions

If unsure, add: // TODO: AVAST security review needed
