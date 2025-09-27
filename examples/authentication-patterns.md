# Authentication Patterns: Vulnerable vs Secure

## Pattern 1: Password Storage

### ❌ Vulnerable (AI-Generated)
```javascript
// Common AI pattern - storing passwords in plaintext
async function createUser(username, password) {
  const user = {
    username: username,
    password: password, // VULNERABLE: Plaintext storage
    created: Date.now()
  };

  await db.users.insert(user);
  return user;
}
```

### ✅ Secure (AVAST-Compliant)
```javascript
// AVAST-compliant password storage with bcrypt
const bcrypt = require('bcrypt');

async function createUser(username, password) {
  const saltRounds = 12; // AVAST: Minimum 10 rounds required
  const hashedPassword = await bcrypt.hash(password, saltRounds);

  const user = {
    username: username,
    password: hashedPassword,
    created: Date.now()
  };

  await db.users.insert(user);

  // AVAST: Never return password hash
  delete user.password;
  return user;
}
```

## Pattern 2: Session Management

### ❌ Vulnerable (AI-Generated)
```python
# Common AI pattern - predictable session tokens
import random

def create_session(user_id):
    # VULNERABLE: Predictable token generation
    session_id = f"session_{user_id}_{random.randint(1000, 9999)}"

    sessions[session_id] = {
        'user_id': user_id,
        'created': datetime.now()
    }

    return session_id
```

### ✅ Secure (AVAST-Compliant)
```python
# AVAST-compliant session management
import secrets
from datetime import datetime, timedelta

def create_session(user_id):
    # AVAST: Cryptographically secure token generation
    session_id = secrets.token_urlsafe(32)

    sessions[session_id] = {
        'user_id': user_id,
        'created': datetime.now(),
        'expires': datetime.now() + timedelta(hours=2),  # AVAST: Session timeout
        'ip_address': request.remote_addr  # AVAST: Session binding
    }

    # AVAST: Regenerate session on privilege change
    invalidate_old_sessions(user_id)

    return session_id
```

## Pattern 3: Authentication Rate Limiting

### ❌ Vulnerable (AI-Generated)
```java
// Common AI pattern - no rate limiting
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request) {
    User user = userService.findByUsername(request.getUsername());

    // VULNERABLE: No rate limiting, allows brute force
    if (user != null && user.getPassword().equals(request.getPassword())) {
        return ResponseEntity.ok(new LoginResponse(generateToken(user)));
    }

    return ResponseEntity.status(401).body("Invalid credentials");
}
```

### ✅ Secure (AVAST-Compliant)
```java
// AVAST-compliant with rate limiting
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request, HttpServletRequest httpRequest) {
    String clientIP = httpRequest.getRemoteAddr();

    // AVAST: Rate limiting (5 attempts per minute)
    if (rateLimiter.isBlocked(clientIP)) {
        return ResponseEntity.status(429).body("Too many attempts. Try again later.");
    }

    User user = userService.findByUsername(request.getUsername());

    if (user == null) {
        // AVAST: Consistent timing to prevent username enumeration
        passwordEncoder.matches("dummy", "$2a$12$dummy.hash");
        rateLimiter.recordFailure(clientIP);
        return ResponseEntity.status(401).body("Invalid credentials");
    }

    if (!passwordEncoder.matches(request.getPassword(), user.getPasswordHash())) {
        rateLimiter.recordFailure(clientIP);
        // AVAST: Log security event
        auditLog.logFailedLogin(request.getUsername(), clientIP);
        return ResponseEntity.status(401).body("Invalid credentials");
    }

    // AVAST: Clear rate limit on success
    rateLimiter.reset(clientIP);

    // AVAST: Regenerate session
    httpRequest.getSession().invalidate();
    HttpSession newSession = httpRequest.getSession(true);

    return ResponseEntity.ok(new LoginResponse(generateToken(user)));
}
```

## Pattern 4: Password Reset

### ❌ Vulnerable (AI-Generated)
```ruby
# Common AI pattern - insecure password reset
def reset_password
  user = User.find_by(email: params[:email])

  if user
    # VULNERABLE: Predictable reset token
    reset_token = user.id.to_s + Time.now.to_i.to_s

    # VULNERABLE: Token stored in plaintext
    user.update(reset_token: reset_token)

    # VULNERABLE: Token sent via email (could be logged)
    UserMailer.password_reset(user, reset_token).deliver_now
  end

  # VULNERABLE: Different response reveals if email exists
  render json: { message: user ? "Reset email sent" : "Email not found" }
end
```

### ✅ Secure (AVAST-Compliant)
```ruby
# AVAST-compliant password reset
def reset_password
  user = User.find_by(email: params[:email])

  # AVAST: Always return same response to prevent enumeration
  render json: { message: "If the email exists, a reset link has been sent" }

  # Process asynchronously to maintain consistent timing
  PasswordResetJob.perform_later(params[:email]) if user
end

class PasswordResetJob < ApplicationJob
  def perform(email)
    user = User.find_by(email: email)
    return unless user

    # AVAST: Cryptographically secure token
    reset_token = SecureRandom.urlsafe_base64(32)

    # AVAST: Hash token before storage
    user.update(
      reset_token_hash: Digest::SHA256.hexdigest(reset_token),
      reset_token_expires: 1.hour.from_now  # AVAST: Token expiration
    )

    # AVAST: Rate limit password resets
    return if user.recent_reset_count > 3

    UserMailer.password_reset(user, reset_token).deliver_now

    # AVAST: Audit log
    AuditLog.create(
      event: 'password_reset_requested',
      user_id: user.id,
      ip: request.remote_ip
    )
  end
end
```

## Pattern 5: Multi-Factor Authentication

### ❌ Vulnerable (AI-Generated)
```go
// Common AI pattern - weak MFA implementation
func verifyMFA(userID string, code string) bool {
    user := getUserByID(userID)

    // VULNERABLE: Comparing as strings, timing attack possible
    if user.MFACode == code {
        // VULNERABLE: No rate limiting on MFA attempts
        return true
    }

    return false
}
```

### ✅ Secure (AVAST-Compliant)
```go
// AVAST-compliant MFA implementation
func verifyMFA(userID string, code string, clientIP string) (bool, error) {
    user := getUserByID(userID)
    if user == nil {
        return false, errors.New("user not found")
    }

    // AVAST: Rate limiting on MFA attempts
    if mfaRateLimiter.IsBlocked(userID, clientIP) {
        auditLog.LogExcessiveMFAAttempts(userID, clientIP)
        return false, errors.New("too many attempts")
    }

    // AVAST: Time-constant comparison
    expectedCode := generateTOTP(user.MFASecret)
    if !subtle.ConstantTimeCompare([]byte(expectedCode), []byte(code)) == 1 {
        mfaRateLimiter.RecordFailure(userID, clientIP)
        auditLog.LogFailedMFA(userID, clientIP)
        return false, errors.New("invalid code")
    }

    // AVAST: Prevent code reuse
    if user.LastMFACode == code {
        auditLog.LogMFACodeReuse(userID, clientIP)
        return false, errors.New("code already used")
    }

    // AVAST: Update last used code
    user.LastMFACode = code
    user.LastMFATime = time.Now()
    updateUser(user)

    // AVAST: Clear rate limit on success
    mfaRateLimiter.Reset(userID, clientIP)

    return true, nil
}
```

## Key Takeaways

1. **AI models consistently miss critical security controls** like rate limiting, secure token generation, and timing attack prevention

2. **AVAST requirements prevent common vulnerabilities** by mandating specific security patterns before code generation

3. **Every authentication operation needs**:
   - Rate limiting
   - Audit logging
   - Secure token generation
   - Session management
   - Timing attack prevention

4. **Test all authentication flows** for these vulnerabilities:
   - Brute force attacks
   - Username enumeration
   - Session fixation
   - Token prediction
   - Timing attacks

## Testing Checklist

- [ ] Password hashing uses bcrypt/argon2 with sufficient rounds
- [ ] Sessions regenerate on login/privilege change
- [ ] Rate limiting prevents brute force
- [ ] Audit logs capture security events
- [ ] Timing attacks are mitigated
- [ ] Tokens are cryptographically secure
- [ ] MFA implementation prevents bypass
- [ ] Password reset doesn't leak information