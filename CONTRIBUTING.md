# Contributing to AVAST Framework

First off, thank you for considering contributing to AVAST! It's people like you that make AVAST such a great tool for securing AI-generated code.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Reporting Vulnerabilities](#reporting-vulnerabilities)
- [Suggesting Enhancements](#suggesting-enhancements)
- [Your First Code Contribution](#your-first-code-contribution)
- [Pull Request Process](#pull-request-process)
- [Style Guides](#style-guides)
- [Community](#community)

## Code of Conduct

This project and everyone participating in it is governed by our commitment to providing a welcoming and inclusive environment. We expect all contributors to:

- Be respectful and considerate
- Welcome newcomers and help them get started
- Focus on constructive criticism
- Show empathy towards other community members

## How Can I Contribute?

### 🐛 Reporting Bugs in AI-Generated Code Patterns

When you discover a new vulnerability pattern in AI-generated code:

1. **Check existing issues** to avoid duplicates
2. **Create a detailed issue** including:
   - The AI tool used (Copilot, Claude, Cursor, etc.)
   - The prompt that triggered the vulnerable code
   - The vulnerable code generated
   - The secure alternative
   - AVAST category (Authentication, Validation, Auditing, Secrets, Trust)

**Example Issue Template:**
```markdown
## AI Vulnerability Pattern Report

**AI Tool:** GitHub Copilot v1.x
**AVAST Category:** Validation

**Prompt Used:**
"Create a function to search users by name"

**Vulnerable Code Generated:**
```javascript
function searchUsers(name) {
  return db.query(`SELECT * FROM users WHERE name = '${name}'`);
}
```

**Secure Alternative:**
```javascript
function searchUsers(name) {
  return db.query('SELECT * FROM users WHERE name = ?', [name]);
}
```

**Impact:** SQL Injection vulnerability
```

### 💡 Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, include:

- **Use case:** Why is this enhancement needed?
- **Expected behavior:** How should it work?
- **Current workaround:** If any

### 📝 Contributing Documentation

Documentation improvements are always welcome! This includes:

- Fixing typos or clarifying existing docs
- Adding examples for different programming languages
- Creating integration guides for new AI tools
- Translating documentation to other languages

### 🔧 Contributing Code

#### Security Rules and Patterns

Add new security patterns to the appropriate instruction files:

1. **For Copilot:** Edit `copilot-instructions.md`
2. **For Cursor/Claude:** Edit `cursor-claude-aider.md`

Format:
```markdown
## [AVAST Category]

### Rule: [Descriptive Name]
**Applies to:** [Languages/Frameworks]
**Security Risk:** [CVE/CWE if applicable]

**Forbidden Pattern:**
```language
// Example of vulnerable code
```

**Required Pattern:**
```language
// Example of secure code
```

**Rationale:** [Why this matters]
```

#### Integration Scripts

Contributing integration scripts for different development environments:

1. Fork the repository
2. Create a new branch: `git checkout -b integration/tool-name`
3. Add your script to `/integrations/`
4. Include documentation in `/docs/integrations/`
5. Submit a pull request

## Your First Code Contribution

Unsure where to begin? Look for these tags in our issues:

- `good-first-issue` - Simple fixes or documentation updates
- `help-wanted` - More involved features needing attention
- `vulnerability-pattern` - New AI vulnerability patterns to document

## Pull Request Process

1. **Fork and Clone**
   ```bash
   git clone https://github.com/your-username/avast-toolkit
   cd avast-toolkit
   git checkout -b feature/your-feature-name
   ```

2. **Make Your Changes**
   - Follow the existing code style
   - Add tests if applicable
   - Update documentation

3. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "feat: Add [feature] to [component]"
   ```

   Commit message format:
   - `feat:` New feature
   - `fix:` Bug fix
   - `docs:` Documentation changes
   - `style:` Code style changes
   - `refactor:` Code refactoring
   - `test:` Test additions/changes
   - `chore:` Maintenance tasks

4. **Push to GitHub**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open Pull Request**
   - Use our PR template
   - Reference any related issues
   - Describe your changes clearly
   - Include examples if relevant

### PR Review Process

- PRs require at least one review
- Address all feedback constructively
- Keep PRs focused and atomic
- Update your branch with main if needed

## Style Guides

### Markdown Style

- Use ATX-style headers (`#` not underlines)
- Include code language identifiers in fenced code blocks
- Prefer lists over long paragraphs
- Include table of contents for long documents

### Code Examples Style

- Always include both vulnerable and secure versions
- Use realistic variable/function names
- Add comments explaining the security issue
- Test all code examples before submitting

### Git Commit Messages

- Use present tense ("Add feature" not "Added feature")
- Limit first line to 72 characters
- Reference issues and PRs in commit body
- Explain *why* not just *what* changed

## Community

### Getting Help

- **GitHub Discussions:** Ask questions and share ideas
- **Issues:** Report bugs and request features
- **Twitter:** Follow [@bradtenenholtz](https://twitter.com/bradtenenholtz) for updates
- **Email:** brad.tenenholtz@gmail.com for security-sensitive discussions

### Recognition

Contributors will be recognized in:
- The project README
- Release notes
- Our contributors page

### Sharing Your Success

If AVAST helped secure your AI-generated code:
1. Share metrics and case studies
2. Write a blog post (we'll help promote it!)
3. Present at meetups or conferences
4. Add your organization to our "Used By" list

## Questions?

Don't hesitate to reach out if you have questions. We're here to help make your contribution successful!

---

Thank you for helping make AI-generated code more secure! 🛡️

*The AVAST Team*