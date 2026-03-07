# Changelog

All notable changes to this project will be documented in this file.

## [3.0.4] - 2026-03-08

### Fixed

- Updated `_meta.json` version field to match published version (was incorrectly showing 3.0.0)
- Updated `CHANGELOG.md` to document 3.0.3 changes

## [3.0.3] - 2026-03-08

### Added

- **`.clawhubignore` file**: Prevents ClawHub/Git conflicts by excluding Git-related files (`.git/`, `.gitignore`, `.github/`) from ClawHub's file comparison. This allows Git repository and ClawHub skill management to coexist without triggering "local changes" errors during updates.

## [3.0.0] - 2026-03-07

### ⚠️ Breaking Changes

- **Code no longer printed to stdout**: `generate_code.sh` now writes the code to a secure state file instead of printing it. Scripts communicate via the state file, not stdout capture.
- **`send_otc_email.sh` API changed**: No longer takes the code as an argument. It reads the code from the state file internally. New usage: `send_otc_email.sh <operation> [session] [lang]`
- **`verify_code.sh` API changed**: No longer takes the expected code as an argument. It reads from the state file internally. New usage: `verify_code.sh <user_input>`

### Security Fixes

- **Eliminated code leakage via stdout**: Code is never printed to stdout by any script. It flows exclusively through a secure state file (mode 600). This prevents the agent from capturing and potentially displaying the code in chat or logs.
- **Cryptographically secure generation**: Replaced `$RANDOM` (predictable PRNG) with `/dev/urandom` for code generation.
- **Removed arbitrary file sourcing**: `send_email_smtp.sh` no longer runs `source ~/.openclaw/.env`. Credentials must be provided via environment variables or OpenClaw config injection.
- **Atomic single-use enforcement**: State file is deleted immediately after successful verification, preventing code reuse.
- **No silent fallbacks**: Email sending failure is always fatal — the script exits with an error and never falls through to allow execution without verification.
- **Custom backend validation**: Custom email scripts are now validated for existence and execute permission before invocation.

### Improved

- **Metadata compliance**: Added `requires.env` and `primaryEnv` to skill metadata, correctly declaring `OTC_EMAIL_RECIPIENT` as a required environment variable.
- **Sanitized documentation**: Removed literal dangerous command examples from trigger categories to avoid false-positive security scanner triggers.
- **Troubleshooting**: Removed credential-exposing debug commands (no more `echo $SMTP_PASS`).
- **Integration examples**: Updated SOUL.md and AGENTS.md integration guides to use the new state-file-based API.
- **Enforcement docs**: Updated checklist and discipline docs to reflect the zero-stdout architecture.

## [2.0.1] - 2026-03-07

### Fixed

- **sed delimiter conflict**: Changed from `/` to `|` to avoid conflicts with operation descriptions containing slashes
- **Automatic language detection**: Email template language now auto-detects based on content

### Changed

- `send_otc_email.sh` now defaults to auto language detection

## [2.0.0] - 2026-03-07

### Added

- Zero-dependency SMTP email sending via curl
- Multiple email backend support: SMTP, send-email, himalaya, custom script
- Standardized scripts (generate, send, verify)
- Email templates with bilingual support and variable substitution
- Configuration examples for environment variables and OpenClaw config
- Enhanced documentation: enforcement checklist, trigger categories, integration guides
- OpenClaw config integration

### Security

- Code secrecy (email-only delivery)
- Single-use codes
- Session binding
- No bypass via natural language
- Email immutability
- Escalation on repeated failures

## [1.0.0] - 2026-03-06

### Added

- Initial release
- Basic OTC confirmation mechanism
- Code generation script
- Core documentation and trigger categories
