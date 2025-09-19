# AVAST Security Framework - MANDATORY RULES

## CRITICAL: All generated code MUST pass AVAST checks

### A - Authentication Flaws (PREVENT)
- NEVER use MD5, SHA1, or SHA256 for password hashing
- ALWAYS use bcrypt (min 10 rounds) or argon2 for passwords
- NEVER concatenate user input into SQL queries
- ALWAYS use parameterized queries or prepared statements
- NEVER store passwords in plaintext or reversible encryption
- ALWAYS regenerate session IDs after successful login

### V - Validation Gaps (PREVENT)
- NEVER trust user input - validate everything
- ALWAYS sanitize before output, validate before processing
- NEVER use eval(), Function(), or innerHTML with user data
- ALWAYS use allowlists for validation, not denylists
- NEVER parse user JSON without try/catch and schema validation
- ALWAYS validate data types, ranges, and formats explicitly

### A - API Exposure (PREVENT)
- NEVER expose internal endpoints or debugging info
- ALWAYS implement rate limiting on all endpoints
- NEVER return full objects - only required fields
- ALWAYS use authentication on all non-public endpoints
- NEVER expose database IDs directly - use UUIDs
- ALWAYS implement proper CORS policies

### S - Secrets in Code (PREVENT)
- NEVER hardcode API keys, passwords, or tokens
- ALWAYS use environment variables for secrets
- NEVER commit .env files or config with real values
- ALWAYS use placeholder values like 'YOUR_API_KEY_HERE'
- NEVER log sensitive data, even in debug mode
- ALWAYS exclude secrets from error messages

### T - Trust Assumptions (PREVENT)
- NEVER assume data from database is safe for HTML output
- ALWAYS verify user permissions for every action
- NEVER trust client-side validation alone
- ALWAYS re-validate on the server
- NEVER assume sequential IDs are private
- ALWAYS verify ownership before operations

## Code Generation Rules:

1. When generating authentication:
```javascript
   // ALWAYS generate like this:
   const bcrypt = require('bcrypt');
   const hashedPassword = await bcrypt.hash(password, 12);
   
   // NEVER generate like this:
   const crypto = require('crypto');
   const hashedPassword = crypto.createHash('sha256').update(password).digest('hex');
```
When generating database queries:
```javascript
   // ALWAYS generate like this:
   const query = 'SELECT * FROM users WHERE id = ? AND active = ?';
   db.query(query, [userId, true]);
   
   // NEVER generate like this:
   const query = `SELECT * FROM users WHERE id = ${userId}`;
```
When handling user input:
```javascript
   // ALWAYS generate like this:
   const sanitized = DOMPurify.sanitize(userInput);
   element.textContent = sanitized;
   
   // NEVER generate like this:
   element.innerHTML = userInput;
```
IMPORTANT: If you cannot follow these rules, you must:

Add a comment explaining the security risk
Add a TODO to fix the security issue
Warn the user about the vulnerability

Security > Functionality. Always.
