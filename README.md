# Docu: Guided Documentation Workflows in VS Code

[![CI](https://github.com/rodhayl/specItGithubCopilot/actions/workflows/ci.yml/badge.svg)](https://github.com/rodhayl/specItGithubCopilot/actions/workflows/ci.yml)
[![VS Code](https://img.shields.io/badge/VS%20Code-1.97%2B-blue.svg)](https://code.visualstudio.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)

**Status: Preview extension, v0.3.0.** Build from source and package as a VSIX using the instructions below. AI-assisted workflows require GitHub Copilot Chat and an available language model.

Docu turns product ideas into editable documentation inside VS Code. Six specialist agents guide product requirements, brainstorming, requirements gathering, architecture, implementation specifications, and review. Conversations, templates, and workspace files connect these steps so you can refine a document without moving between a chat window and a separate authoring tool.

This repository, `specItGithubCopilot`, contains the Docu extension. Its engineering focus is conversational document workflows: agent routing, session state, template rendering, and Markdown file updates through VS Code's Chat Participant and Language Model APIs.

## What you can do

- Start a document from a plain-language request or a slash command
- Refine the same document across conversation turns
- Switch between six documentation roles and track workflow phases
- Use built-in templates or add workspace-specific templates
- Create, open, and update Markdown documents in the workspace
- Run document review and inspect diagnostics while you work

Generated content needs human review. Check requirements, technical decisions, and file changes before relying on them.

## Quick start

### Requirements

- VS Code **1.97.0 or later**, as declared in [package.json](package.json)
- GitHub Copilot Chat installed and enabled, with access to a compatible chat model
- An open workspace folder where Docu can create documents
- Git, Node.js, and npm for building from source; repository CI uses **Node.js 20**

### Build and install

```bash
git clone https://github.com/rodhayl/specItGithubCopilot.git
cd specItGithubCopilot
npm ci
npm run compile
npm run package
```

`npm run package` creates the VSIX; compilation alone does not. In VS Code, run **Extensions: Install from VSIX...** from the Command Palette and select the generated file. With the current package name and version, the filename is `vscode-docu-extension-0.3.0.vsix`.

Alternatively, with the VS Code command-line tool available:

```bash
code --install-extension vscode-docu-extension-0.3.0.vsix
```

Use the filename produced by your build. Some detailed guides still contain older version examples; the manifest and build output determine the current filename. See the [installation guide](docs/installation.md) for additional setup information.

### Create your first document

1. Open a workspace folder and GitHub Copilot Chat
2. Select an available model in the chat toolbar
3. Send a request to `@docu`, for example:

```text
@docu I want to build a task management app with real-time collaboration
```

The natural-language session classifies the request, starts a draft, opens the document, and asks a follow-up question. Continue replying to refine the same file. Type `done`, `finish`, or `/done` to close the session.

For an explicit template-based start:

```text
@docu /new "My Product Requirements" --template prd
```

Review the created file and keep changes under version control. Auto-save and conversation-driven updates are enabled by default.

## Agents and workflow

| Agent | Role |
| --- | --- |
| `prd-creator` | Product concept, goals, users, and PRD drafting |
| `brainstormer` | Ideation and concept expansion |
| `requirements-gatherer` | Functional and non-functional requirements, including EARS-style wording |
| `solution-architect` | Architecture and technical design |
| `specification-writer` | Implementation specifications and task planning |
| `quality-reviewer` | Document checks and improvement suggestions |

The workflow moves through **PRD → requirements → design → implementation planning and review**. You can select a specialist directly rather than completing every phase.

Natural-language document sessions use these folders:

```text
docs/
  prd/
  requirements/
  design/
  spec/
  ideas/
```

Other document commands support the default-directory setting or an explicit path. See the [agent guide](docs/agents.md) and [example workflows](examples/) for longer walkthroughs.

## Usage guide

### Documents

```text
@docu /new "Document Title"
@docu /new "API Docs" --template basic
@docu /new "User Guide" --path docs/guides/user-guide.md
```

### Agents and templates

```text
@docu /agent list
@docu /agent set requirements-gatherer
@docu /agent current
@docu /templates list
@docu /templates show prd
@docu /templates validate my-template
```

### Updates and review

Replace the example paths with an existing document in your workspace:

```text
@docu /update --file docs/api.md --section "Authentication" --mode append "New auth notes"
@docu /review --file docs/requirements.md --level strict
```

`/update` supports `replace`, `append`, and `prepend`. Review supports `light`, `normal`, and `strict` levels; its optional `--fix` flag can modify the document. Inspect the resulting diff, particularly when applying automatic fixes.

Use `@docu /help` for available commands. The [command reference](docs/command-reference.md), [complete tutorial](docs/complete-tutorial.md), and [demo project](examples/demo-project/) provide more examples.

## Configuration

Open VS Code Settings and search for `docu`. The complete setting definitions are in [package.json](package.json).

| Setting | Default | Purpose |
| --- | --- | --- |
| `docu.defaultDirectory` | `docs` | Default directory for document commands |
| `docu.defaultAgent` | `prd-creator` | Startup agent |
| `docu.templateDirectory` | `.vscode/docu/templates` | Workspace custom-template directory |
| `docu.autoSaveDocuments` | `true` | Auto-save created or updated documents |
| `docu.autoChat.enableDocumentUpdates` | `true` | Allow document updates during conversations |
| `docu.showWorkflowProgress` | `true` | Show workflow transitions in chat |
| `docu.logging.level` | `info` | Log level: `debug`, `info`, `warn`, `error`, or `none` |
| `docu.telemetry.enabled` | `true` | Collect diagnostic events, subject to VS Code telemetry settings |
| `docu.telemetry.anonymizeData` | `true` | Apply the implemented identifier hashing and redaction rules |
| `docu.debug.autoStart` | `false` | Start the local debug HTTP server automatically |

### Custom templates

Add a Markdown file such as `.vscode/docu/templates/my-template.md` with YAML frontmatter and template variables:

```markdown
---
id: my-template
name: My Custom Template
description: A template for project notes
variables:
  - name: title
    description: Document title
    required: true
    type: string
---

# {{title}}

Created: {{created}}

## Overview

Add the project context here.
```

See [template management](docs/template-management.md) for the template format and editing commands.

## Data, file changes, and limits

### Workspace files and model requests

Documents are written to the open workspace. AI requests use VS Code's language-model integration with GitHub Copilot and can include your prompt and document content. Review the content you provide and your applicable Copilot settings and policies before using confidential material.

The source includes path checks, allowed-extension and blocked-directory rules, file-size checks, and input-sanitization helpers. These are implementation controls, not a guarantee that every input or operation is safe. Use a trusted workspace and inspect generated files and automatic changes.

### Diagnostics and telemetry

Diagnostic event collection is enabled by default and respects `vscode.env.isTelemetryEnabled`. The current [TelemetryManager](src/telemetry/TelemetryManager.ts) keeps events in memory and provides a JSON export; that class does not implement remote event transmission. This is separate from content sent to a language model.

To disable Docu's diagnostic event collection, set:

```json
{
  "docu.telemetry.enabled": false
}
```

Hashing and redaction do not guarantee that logs or exported diagnostics contain no identifying or sensitive information. Review reports before sharing them. Logging has separate settings from telemetry.

### When a model is unavailable

The extension includes non-AI file and template operations and fallback messages. AI drafting, refinement, and model-based assistance require an available model; offline support does not provide a local language model. Availability and fallback behavior should be checked in your VS Code environment.

### Troubleshooting

The Command Palette includes:

- **Docu: Show Diagnostics**
- **Docu: Export Diagnostics**
- **Docu: Show Output Channel**
- **Docu: Toggle Debug Mode**
- **Docu: Check Offline Mode Status**

The optional debug HTTP server is a development tool with command-execution capabilities. Leave it disabled unless you need it for debugging. See the [troubleshooting guide](docs/troubleshooting.md) and [security policy](SECURITY.md).

## Development and validation

```bash
npm run compile        # Compile TypeScript
npm run watch          # Recompile while developing
npm run lint           # Type-check without emitting files
npm test               # Compile, type-check, and run Jest
npm run test:coverage  # Run Jest with coverage reporting
npm run package        # Package the extension as a VSIX
```

The [CI workflow](.github/workflows/ci.yml) installs dependencies with `npm ci`, runs TypeScript checks, and runs Jest on Node.js 20. See the CI badge for the reported result.

The repository includes unit, integration, and workflow tests. Some integration scenarios, including online/offline conversations, use mocks. A passing Jest run does not establish the behavior of every live Copilot session. Use the [testing guide](docs/testing.md) and [manual test checklist](MANUAL_TEST.md) for interactive verification in VS Code, and record the commit and test scope when sharing results.

### Source map

```text
src/
  agents/        # Specialist agents and agent selection
  commands/      # Parsing, routing, and workflow commands
  config/        # Settings and configuration
  conversation/  # Sessions, state transitions, and document refinement
  llm/           # Language-model integration and prompts
  templates/     # Template loading and rendering
  tools/         # File and document tools
  security/      # Workspace and input validation helpers
  telemetry/     # Diagnostic event collection and export
tests/           # Jest suites and supporting mocks
docs/            # Guides and command reference
examples/        # Sample documents and workflows
```

## Project history

Recorded development dates to August 2025, starting from a VS Code extension scaffold and expanding into documentation agents, conversation state, templates, and file tools. The current public repository starts with a [separate initial commit dated 25 February 2026](https://github.com/rodhayl/specItGithubCopilot/commit/eb7f1a232e78414bd566a8f40843edefae797707). That date identifies this repository's initial source snapshot, rather than the start of the project's development.

## Documentation and contributing

- [Documentation index](docs/README.md)
- [Quick-start guide](docs/quick-start.md)
- [Compilation guide](docs/compilation-guide.md)
- [FAQ](docs/faq.md)
- [Contributing guide](CONTRIBUTING.md)
- [Report an issue](https://github.com/rodhayl/specItGithubCopilot/issues)

## License

[MIT](LICENSE.md) · Copyright (c) 2026 rodhayl
