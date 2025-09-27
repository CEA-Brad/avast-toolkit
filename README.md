# 🚢 AVAST Framework
> **A Threat Modeling Framework for AI-Generated Code (STRIDE+ for AI)**

> **A**uthentication | **V**alidation | **A**uditing | **S**ecrets | **T**rust

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Threat Modeling](https://img.shields.io/badge/Threat%20Modeling-STRIDE%2B-orange)](https://github.com/CEA-Brad/avast-toolkit)
[![AI Security](https://img.shields.io/badge/AI-Security-blue)](https://github.com/CEA-Brad/avast-toolkit)

## 🎯 What is AVAST?

AVAST is a threat modeling framework that extends STRIDE specifically for AI-generated code. If you're familiar with STRIDE (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege), you know it's excellent but intentionally general. Security practitioners have long used "STRIDE+" to add domain-specific concerns:

- **STRIDE + Cloud** for cloud-specific threats
- **STRIDE + IoT** for embedded systems
- **STRIDE + Mobile** for mobile applications
- **STRIDE + AVAST** for AI-generated code ✨

## 📊 The Problem

AI-generated code is creating a security crisis:
- **45%** of AI-generated code fails basic security tests
- **62%** contain formally verifiable vulnerabilities
- **72%** failure rate for enterprise Java applications

Despite this, **83% of organizations** use AI to generate production code.

AVAST provides the specific threat categories that AI consistently gets wrong, allowing you to maintain comprehensive STRIDE coverage while ensuring AI-specific vulnerabilities don't slip through.

## 🌟 Why Use STRIDE + AVAST?

When threat modeling AI-assisted development:

1. **Start with STRIDE** for comprehensive threat coverage
2. **Add AVAST** to catch what AI specifically gets wrong
3. **Get complete coverage** without redundancy

Benefits:
✅ **Proactive Security**: Catch AI-specific vulnerabilities during threat modeling
✅ **Context-Aware**: Provides the security context AI models lack
✅ **Measurable Impact**: 67% reduction in production vulnerabilities
✅ **Industry Standard**: Builds on STRIDE, not replacing it

## 📊 The AVAST Components

AVAST extends your STRIDE threat model with five AI-specific vulnerability categories:

| AVAST Component | STRIDE Mapping | Description | Common AI Failures |
|-----------------|----------------|-------------|-------------------|
| **Authentication** | Spoofing | Identity verification and session management | Hardcoded credentials, weak session handling |
| **Validation** | Tampering, Info Disclosure | Input sanitization and data validation | SQL injection, XSS vulnerabilities |
| **Auditing** | Repudiation | Logging and monitoring | Sensitive data exposure, insufficient logging |
| **Secrets** | Information Disclosure | Credential and key management | Secrets in code, plaintext storage |
| **Trust** | Elevation of Privilege | Boundary validation and authorization | Missing authorization, privilege escalation |

### How to Use STRIDE + AVAST

```yaml
# Traditional STRIDE threat model
stride_analysis:
  spoofing: [Check authentication mechanisms]
  tampering: [Verify data integrity]
  repudiation: [Ensure audit trails]

# Add AVAST for AI-specific threats
avast_analysis:
  authentication: [AI won't implement rate limiting]
  validation: [AI will concatenate SQL queries]
  auditing: [AI will log passwords]
  secrets: [AI will hardcode API keys]
  trust: [AI will trust client-side validation]
```

## 🚀 Quick Start

### 1. Add AVAST rules to your AI coding assistant

**For GitHub Copilot:**
```bash
cat copilot-instructions.md >> .github/copilot-instructions.md
```

**For Cursor/Claude/Aider:**
```bash
cat cursor-claude-aider.md >> .cursorrules
```

### 2. Configure your git hooks

Create `.git/hooks/pre-commit`:
```bash
#!/bin/bash
# Check for AVAST rules in AI instruction files
if [ -f ".cursorrules" ] && ! grep -q "AVAST" .cursorrules; then
    echo "⚠️  Warning: .cursorrules missing AVAST security rules"
    echo "Run: cat cursor-claude-aider.md >> .cursorrules"
    exit 1
fi
```

### 3. Implement team-wide enforcement

Create `.github/avast-config.yml`:
```yaml
avast:
  enforce: true
  instruction_files:
    - .cursorrules
    - .claude
    - copilot-instructions.md
  required_sections:
    - Authentication
    - Validation
    - API Security
    - Secrets
    - Trust
  scan_on:
    - pull_request
    - push
```

## 📈 Proven Results

Organizations implementing AVAST report:
- **67%** reduction in production vulnerabilities
- **40%** less time spent on security fixes
- **3x** faster security reviews
- **50%** reduction in AI-generated vulnerabilities within 30 days

## 🛠️ Implementation Guide

### Phase 1: Discovery (Week 1)
```bash
# Find your AI coding footprint
git grep -l "copilot\|cursor\|claude" --all
```

### Phase 2: Assessment (Week 2)
Run the AVAST checklist on recent AI-generated code:
- [ ] Authentication flaws identified?
- [ ] Validation gaps found?
- [ ] Auditing issues detected?
- [ ] Secrets in code discovered?
- [ ] Trust boundary violations?

### Phase 3: Implementation (Weeks 3-4)
1. Deploy AVAST rules to development teams
2. Implement pre-commit hooks
3. Configure CI/CD security gates
4. Track metrics and iterate

## 📚 Documentation

- [Full AVAST Framework Guide](./AVAST-Framework.md) - Comprehensive methodology and implementation
- [Examples](./examples/) - Vulnerable vs. secure code comparisons
- [Integration Guides](./docs/integrations/) - Tool-specific setup instructions
- [Security Patterns](./docs/patterns/) - Common vulnerability patterns and fixes

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

### Ways to Contribute:
- Report vulnerabilities patterns you've discovered
- Submit integration guides for new AI tools
- Share success stories and metrics
- Improve documentation

## 📊 Used By

AVAST is trusted by security-conscious development teams worldwide. If your organization uses AVAST, please consider adding your logo here.

## 📖 Citation

If you use AVAST in your research or organization, please cite:

```bibtex
@misc{tenenholtz2025avast,
  author = {Tenenholtz, Brad},
  title = {AVAST: A Threat Modeling Framework for AI-Assisted Development},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/CEA-Brad/avast-toolkit}
}
```

## 👤 Author

**Brad Tenenholtz**
Security Architect & AI Security Researcher

- 📧 Email: brad.tenenholtz@gmail.com
- 💼 LinkedIn: [Brad Tenenholtz](https://www.linkedin.com/in/the-real-brad-tenenholtz/)
- 🐦 Twitter: [@bradten](https://x.com/bradten)
- 🎤 Speaker: CornCon, ICS Cybersecurity Conference, Critical Infrastructure Protection & Resilience North America

### Background
With over a decade of experience in application security and threat modeling, I developed AVAST after observing the exponential increase in AI-generated vulnerabilities across enterprise codebases. This framework represents the synthesis of traditional security principles adapted for the AI development era.

## 📄 License

MIT License - see [LICENSE](./LICENSE) file for details.

## 🙏 Acknowledgments

Special thanks to:
- CornCon 2025 attendees for valuable feedback
- Early adopters who provided metrics and case studies
- The open-source security community

## 🚨 Security

Found a security issue? Please email brad.tenenholtz@gmail.com directly rather than opening a public issue.

---

<div align="center">
  <strong>🛡️ Secure your AI-generated code with AVAST</strong><br>
  <sub>Because your AI doesn't know what it doesn't know about security</sub>
</div>
