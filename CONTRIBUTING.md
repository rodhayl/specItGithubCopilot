# Contributing to Docu

Thank you for contributing to Docu. This guide explains how to set up your environment, submit changes, and keep contributions review-ready.

## Development Setup

Prerequisites:
- VS Code 1.97+
- Node.js 20+
- npm 10+
- GitHub Copilot Chat (for feature validation)

Clone and install:

```bash
git clone https://github.com/rodhayl/specItGithubCopilot.git
cd specItGithubCopilot
npm install
npm run compile
```

Run in VS Code:
- Open the repository in VS Code
- Press `F5` to launch the Extension Development Host

## Workflow

1. Create a branch from `main`.
2. Make focused changes (single concern per PR).
3. Run local checks before pushing.
4. Open a pull request with clear context and testing notes.

Example branch names:
- `feat/template-validation-improvements`
- `fix/command-router-null-context`
- `docs/installation-guide-refresh`

## Local Quality Gates

Run these before opening a PR:

```bash
npm run compile
npm run lint
npm test -- --runInBand
```

Useful additional commands:

```bash
npm run test:coverage
npm run test:conversation-flow
npm run package
```

## Code Standards

- Use TypeScript with explicit types for public interfaces.
- Keep modules cohesive; avoid large cross-cutting edits in one PR.
- Preserve existing architecture boundaries (`agents`, `commands`, `conversation`, `tools`, etc.).
- Add tests for behavior changes and regressions.
- Keep user-facing messages actionable and concise.

## Pull Request Requirements

Each PR should include:
- Problem statement
- Summary of implementation
- Test evidence (commands and outcomes)
- Screenshots or logs when UI/interaction changes are involved

Checklist:
- [ ] Code compiles
- [ ] Lint/type-check passes
- [ ] Tests pass locally
- [ ] Docs updated (if behavior changed)
- [ ] No secrets or local artifacts included

## Reporting Issues

Use GitHub Issues for bugs and enhancements:
- https://github.com/rodhayl/specItGithubCopilot/issues

For security issues, do not open a public issue. Follow [SECURITY.md](SECURITY.md).

## License

By contributing, you agree that your contributions are licensed under the MIT License.
