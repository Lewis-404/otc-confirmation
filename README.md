# OTC Confirmation

One-Time Confirmation code security mechanism for sensitive agent operations.

## Features

- 🔐 **Zero-knowledge code flow** — code never appears in stdout, logs, or agent context
- 🔒 **Cryptographically secure** — uses `/dev/urandom` for code generation
- 🛡️ **Atomic single-use** — state file deleted on successful verification
- 🚫 **No silent fallbacks** — email failure blocks execution, never falls through
- 📧 **Zero dependencies** — built-in SMTP support via curl
- 🔌 **Multiple backends** — SMTP, send-email, himalaya, or custom scripts
- ⚙️ **Easy configuration** — via openclaw.json or environment variables
- 📝 **Comprehensive docs** — enforcement checklist, trigger categories, integration guides
- 🌐 **Bilingual** — English and Chinese support

## Quick Start

```bash
# Install
clawhub install otc-confirmation

# Configure (add to openclaw.json)
{
  "skills": {
    "entries": {
      "otc-confirmation": {
        "enabled": true,
        "env": {
          "OTC_EMAIL_RECIPIENT": "user@example.com",
          "OTC_EMAIL_BACKEND": "smtp",
          "OTC_SMTP_HOST": "smtp.gmail.com",
          "OTC_SMTP_PORT": "587",
          "OTC_SMTP_USER": "your-email@gmail.com",
          "OTC_SMTP_PASS": "your-app-password"
        }
      }
    }
  }
}

# Use in your agent (code never appears in stdout!)
bash scripts/generate_code.sh                          # → stored in state file
bash scripts/send_otc_email.sh "Operation" "Session"   # → reads from state file
bash scripts/verify_code.sh "$USER_INPUT"              # → exit 0 = verified
```

## Security Model

The OTC code flows exclusively through a secure state file (mode 600):

1. `generate_code.sh` → writes code to state file, nothing to stdout
2. `send_otc_email.sh` → reads from state file, sends email
3. `verify_code.sh` → reads from state file, compares, deletes on match

The agent **never captures or sees the code** in its context. It only checks exit codes.

## Documentation

- **SKILL.md** — Complete usage guide
- **CHANGELOG.md** — Version history
- **references/enforcement-checklist.md** — Step-by-step enforcement workflow
- **references/trigger-categories.md** — When to trigger OTC
- **examples/soul_md_integration.md** — Integrate into SOUL.md
- **examples/agents_md_integration.md** — Integrate into AGENTS.md

## Version

Current: 3.0.2

## License

MIT

## Author

Lewis-404
