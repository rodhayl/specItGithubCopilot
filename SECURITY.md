# Security Policy

## Supported Versions

| Version | Supported |
| --- | --- |
| 0.3.x | Yes |
| < 0.3.0 | No |

## Reporting a Vulnerability

Do not disclose vulnerabilities publicly in issues or discussions.

Use one of these channels:
- GitHub Private Vulnerability Reporting: `Security` tab in this repository
- Direct maintainer contact through GitHub profile

Please include:
- A clear description of the issue and impact
- Reproduction steps or proof-of-concept
- Environment details (OS, VS Code version, extension version)
- Suggested mitigation, if available

## Response Targets

- Acknowledgement: within 48 hours
- Initial triage: within 5 business days
- Fix or mitigation timeline: based on severity

## Security Scope

This extension interacts with:
- VS Code Extension API
- GitHub Copilot Chat language model APIs
- Local workspace files (scoped and validated)

This extension does not:
- Store user credentials
- Access files outside the validated workspace scope
- Make arbitrary outbound network calls for user data collection
