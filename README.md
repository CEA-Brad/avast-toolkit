# avast-toolkit
Agentic rules snippets and other tools for the AVAST AI-aware threat modelling framework

Implementation Strategy

Add to existing instruction files:
```bash
bashcat avast-rules.md >> .cursorrules
```
Create a git hook to ensure they're present:
```
bash#!/bin/bash
# Check for AVAST rules in AI instruction files
if [ -f ".cursorrules" ] && ! grep -q "AVAST" .cursorrules; then
    echo "⚠️  .cursorrules missing AVAST rules"
fi
```
Team-wide rollout:
```
yaml# .github/avast-config.yml
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
```
