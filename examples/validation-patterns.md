# Validation Patterns: Vulnerable vs Secure

## Pattern 1: SQL Injection

### ❌ Vulnerable (AI-Generated)
```javascript
// Common AI pattern - string concatenation for queries
async function searchProducts(searchTerm, category) {
  // VULNERABLE: Direct string concatenation
  const query = `
    SELECT * FROM products
    WHERE name LIKE '%${searchTerm}%'
    AND category = '${category}'
  `;

  return await db.raw(query);
}
```

### ✅ Secure (AVAST-Compliant)
```javascript
// AVAST-compliant parameterized queries
async function searchProducts(searchTerm, category) {
  // AVAST: Input validation first
  if (!isValidSearchTerm(searchTerm) || !isValidCategory(category)) {
    throw new ValidationError('Invalid input');
  }

  // AVAST: Parameterized query
  const query = `
    SELECT * FROM products
    WHERE name LIKE ?
    AND category = ?
  `;

  // AVAST: Sanitize wildcards in LIKE queries
  const safeTerm = `%${searchTerm.replace(/[%_]/g, '\\$&')}%`;

  return await db.raw(query, [safeTerm, category]);
}

function isValidSearchTerm(term) {
  // AVAST: Whitelist validation
  return /^[a-zA-Z0-9\s\-]{1,100}$/.test(term);
}

function isValidCategory(category) {
  // AVAST: Enum validation
  const validCategories = ['electronics', 'clothing', 'books', 'home'];
  return validCategories.includes(category);
}
```

## Pattern 2: Cross-Site Scripting (XSS)

### ❌ Vulnerable (AI-Generated)
```python
# Common AI pattern - direct HTML rendering
@app.route('/profile/<username>')
def profile(username):
    user = get_user(username)

    # VULNERABLE: Direct HTML interpolation
    html = f"""
    <h1>Welcome {user.name}!</h1>
    <p>Bio: {user.bio}</p>
    <div>Last message: {request.args.get('msg', '')}</div>
    """

    return html
```

### ✅ Secure (AVAST-Compliant)
```python
# AVAST-compliant XSS prevention
from flask import render_template, escape
from markupsafe import Markup

@app.route('/profile/<username>')
def profile(username):
    # AVAST: Input validation
    if not is_valid_username(username):
        abort(400, 'Invalid username')

    user = get_user(username)

    # AVAST: Context-aware output encoding
    context = {
        'name': escape(user.name),
        'bio': escape(user.bio),
        'message': escape(request.args.get('msg', ''))
    }

    # AVAST: Use templating engine with auto-escaping
    return render_template('profile.html', **context)

def is_valid_username(username):
    # AVAST: Whitelist validation
    return re.match(r'^[a-zA-Z0-9_-]{3,20}$', username) is not None

# Template file (profile.html) with auto-escaping
"""
<!DOCTYPE html>
<html>
<body>
    <h1>Welcome {{ name }}!</h1>
    <p>Bio: {{ bio }}</p>
    {% if message %}
        <div>Last message: {{ message }}</div>
    {% endif %}
</body>
</html>
"""
```

## Pattern 3: Command Injection

### ❌ Vulnerable (AI-Generated)
```ruby
# Common AI pattern - direct command execution
def process_image(filename, resize_option)
  # VULNERABLE: Direct command interpolation
  command = "convert #{filename} -resize #{resize_option} output.jpg"
  system(command)

  return "output.jpg"
end
```

### ✅ Secure (AVAST-Compliant)
```ruby
# AVAST-compliant command execution
require 'shellwords'

def process_image(filename, resize_option)
  # AVAST: Input validation
  validate_filename!(filename)
  validate_resize_option!(resize_option)

  # AVAST: Use array form to prevent shell interpretation
  command = [
    'convert',
    filename,
    '-resize',
    resize_option,
    'output.jpg'
  ]

  # AVAST: Use safe execution method
  success = system(*command)

  # AVAST: Error handling
  raise ProcessingError, 'Image conversion failed' unless success

  return 'output.jpg'
end

def validate_filename!(filename)
  # AVAST: Whitelist validation for filenames
  unless filename.match?(/\A[\w\-\.]+\.(jpg|jpeg|png|gif)\z/i)
    raise ValidationError, 'Invalid filename'
  end

  # AVAST: Ensure file exists and is readable
  unless File.exist?(filename) && File.readable?(filename)
    raise ValidationError, 'File not accessible'
  end
end

def validate_resize_option!(option)
  # AVAST: Strict format validation
  unless option.match?(/\A\d{1,4}x\d{1,4}\z/)
    raise ValidationError, 'Invalid resize dimensions'
  end
end
```

## Pattern 4: Path Traversal

### ❌ Vulnerable (AI-Generated)
```java
// Common AI pattern - unchecked file access
@GetMapping("/download")
public ResponseEntity<Resource> downloadFile(@RequestParam String filename) {
    // VULNERABLE: Direct path concatenation
    Path filePath = Paths.get("/uploads/" + filename);
    Resource resource = new FileSystemResource(filePath);

    return ResponseEntity.ok()
        .header(HttpHeaders.CONTENT_DISPOSITION,
                "attachment; filename=\"" + filename + "\"")
        .body(resource);
}
```

### ✅ Secure (AVAST-Compliant)
```java
// AVAST-compliant file access
@GetMapping("/download")
public ResponseEntity<Resource> downloadFile(@RequestParam String filename) {
    // AVAST: Input validation
    if (!isValidFilename(filename)) {
        throw new ValidationException("Invalid filename");
    }

    // AVAST: Normalize and validate path
    Path basePath = Paths.get("/uploads").toAbsolutePath().normalize();
    Path filePath = basePath.resolve(filename).normalize();

    // AVAST: Ensure path is within intended directory
    if (!filePath.startsWith(basePath)) {
        throw new SecurityException("Path traversal attempt detected");
    }

    // AVAST: Check file existence and readability
    File file = filePath.toFile();
    if (!file.exists() || !file.isFile() || !file.canRead()) {
        throw new NotFoundException("File not found");
    }

    // AVAST: Validate file size
    if (file.length() > MAX_FILE_SIZE) {
        throw new ValidationException("File too large");
    }

    Resource resource = new FileSystemResource(file);

    // AVAST: Sanitize filename for header
    String safeFilename = filename.replaceAll("[^a-zA-Z0-9._-]", "_");

    return ResponseEntity.ok()
        .header(HttpHeaders.CONTENT_DISPOSITION,
                "attachment; filename=\"" + safeFilename + "\"")
        .header(HttpHeaders.CONTENT_TYPE,
                determineContentType(filename))
        .body(resource);
}

private boolean isValidFilename(String filename) {
    // AVAST: Whitelist validation
    return filename != null
        && filename.matches("^[a-zA-Z0-9._-]{1,255}$")
        && !filename.contains("..")
        && !filename.contains("/")
        && !filename.contains("\\");
}
```

## Pattern 5: XML/XXE Injection

### ❌ Vulnerable (AI-Generated)
```csharp
// Common AI pattern - unsafe XML parsing
public User ParseUserXml(string xmlContent)
{
    // VULNERABLE: XXE attacks possible
    var doc = new XmlDocument();
    doc.LoadXml(xmlContent);

    var user = new User
    {
        Name = doc.SelectSingleNode("//name").InnerText,
        Email = doc.SelectSingleNode("//email").InnerText
    };

    return user;
}
```

### ✅ Secure (AVAST-Compliant)
```csharp
// AVAST-compliant XML parsing
public User ParseUserXml(string xmlContent)
{
    // AVAST: Input size validation
    if (xmlContent.Length > MAX_XML_SIZE)
    {
        throw new ValidationException("XML content too large");
    }

    // AVAST: Safe XML parser settings
    var settings = new XmlReaderSettings
    {
        DtdProcessing = DtdProcessing.Prohibit,  // AVAST: Disable DTD
        XmlResolver = null,                      // AVAST: Disable external references
        MaxCharactersFromEntities = 1024,        // AVAST: Limit entity expansion
        MaxCharactersInDocument = 10000
    };

    using (var stringReader = new StringReader(xmlContent))
    using (var xmlReader = XmlReader.Create(stringReader, settings))
    {
        var doc = new XmlDocument();
        doc.Load(xmlReader);

        // AVAST: Validate against schema
        ValidateAgainstSchema(doc);

        // AVAST: Safe node access with validation
        var nameNode = doc.SelectSingleNode("//name");
        var emailNode = doc.SelectSingleNode("//email");

        if (nameNode == null || emailNode == null)
        {
            throw new ValidationException("Required fields missing");
        }

        var user = new User
        {
            Name = SanitizeName(nameNode.InnerText),
            Email = ValidateEmail(emailNode.InnerText)
        };

        return user;
    }
}

private string SanitizeName(string name)
{
    // AVAST: Whitelist validation
    if (!Regex.IsMatch(name, @"^[a-zA-Z\s'-]{1,100}$"))
    {
        throw new ValidationException("Invalid name format");
    }
    return name.Trim();
}

private string ValidateEmail(string email)
{
    // AVAST: Email validation
    try
    {
        var addr = new System.Net.Mail.MailAddress(email);
        return addr.Address;
    }
    catch
    {
        throw new ValidationException("Invalid email format");
    }
}
```

## Pattern 6: NoSQL Injection

### ❌ Vulnerable (AI-Generated)
```javascript
// Common AI pattern - unsafe NoSQL query construction
app.post('/login', async (req, res) => {
  const { username, password } = req.body;

  // VULNERABLE: Direct object interpolation
  const user = await db.collection('users').findOne({
    username: username,
    password: password
  });

  if (user) {
    res.json({ token: generateToken(user) });
  } else {
    res.status(401).json({ error: 'Invalid credentials' });
  }
});
```

### ✅ Secure (AVAST-Compliant)
```javascript
// AVAST-compliant NoSQL query
app.post('/login', async (req, res) => {
  const { username, password } = req.body;

  // AVAST: Input validation and sanitization
  if (typeof username !== 'string' || typeof password !== 'string') {
    return res.status(400).json({ error: 'Invalid input type' });
  }

  // AVAST: Prevent operator injection
  const sanitizedUsername = username.replace(/[$]/g, '');

  // AVAST: Length validation
  if (sanitizedUsername.length > 50 || password.length > 100) {
    return res.status(400).json({ error: 'Input too long' });
  }

  // AVAST: Use explicit equality operator
  const user = await db.collection('users').findOne({
    username: { $eq: sanitizedUsername }
  });

  // AVAST: Never compare passwords directly
  if (user && await bcrypt.compare(password, user.passwordHash)) {
    // AVAST: Remove sensitive fields
    delete user.passwordHash;
    res.json({ token: generateToken(user) });
  } else {
    // AVAST: Consistent error messages
    res.status(401).json({ error: 'Invalid credentials' });
  }
});
```

## Key Takeaways

1. **Never trust user input** - Always validate and sanitize
2. **Use parameterized queries** - Never concatenate user input into queries
3. **Context-aware encoding** - Different contexts require different encoding
4. **Whitelist validation** - Define what's allowed, reject everything else
5. **Defense in depth** - Multiple layers of validation
6. **Fail securely** - Errors should not expose system information

## Testing Checklist

- [ ] All database queries use parameterization
- [ ] User input is validated against whitelists
- [ ] Output is encoded based on context
- [ ] File operations validate paths
- [ ] XML parsing disables dangerous features
- [ ] Command execution uses safe methods
- [ ] Error messages don't leak information
- [ ] Input length limits are enforced