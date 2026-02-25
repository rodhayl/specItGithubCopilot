# Docu - AI Documentation Assistant

[![VS Code](https://img.shields.io/badge/VS%20Code-1.97%2B-blue.svg)](https://code.visualstudio.com/)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-required-brightgreen.svg)](https://github.com/features/copilot)
[![CI](https://github.com/rodhayl/specItGithubCopilot/actions/workflows/ci.yml/badge.svg)](https://github.com/rodhayl/specItGithubCopilot/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)
[![Version](https://img.shields.io/badge/version-0.3.0-orange.svg)](CHANGELOG.md)

A VS Code extension that provides AI-powered documentation assistance through GitHub Copilot Chat. Docu helps you create, manage, and evolve high-quality software documentation using specialized AI agents and guided workflows — from initial product ideas through to detailed technical specifications.

---

## Features

- **Six Specialized AI Agents** — Each agent is tailored for a specific documentation phase: PRD, brainstorming, requirements, architecture, specification, and quality review
- **Smart Templates** — Built-in and fully customizable templates for every document type
- **Guided Workflow** — Structured progression from concept (PRD) through requirements, design, and implementation
- **Slash Commands** — Powerful command system (`/new`, `/agent`, `/templates`, `/review`, `/update`) for quick document operations
- **Natural-Language Sessions** — Start iterative document workflows with plain text; no slash commands required
- **Security and Privacy** — Workspace isolation, path validation, and input sanitization on all operations
- **Offline Support** — Graceful degradation when AI features are unavailable

---

## Quick Start

### Prerequisites

- VS Code **1.97.0** or higher
- GitHub Copilot (with Chat) — active subscription required

### Installation

Install from the `.vsix` file or build from source:

```bash
git clone https://github.com/rodhayl/specItGithubCopilot.git
cd specItGithubCopilot
npm install
npm run compile
```

Then in VS Code: **Extensions** -> `...` -> **Install from VSIX** -> select the built `.vsix` file.

See [docs/installation.md](docs/installation.md) for full details.

### First Steps

1. Open GitHub Copilot Chat (`Ctrl+Shift+I` / `Cmd+Shift+I`)
2. Type `@docu` to start interacting with the assistant
3. Create your first document:

```
@docu /new "My Product Requirements" --template prd
```

Or just describe what you want to build in plain language:

```
@docu I want to build a task management app with real-time collaboration
```

Docu will classify the intent, assign the right specialist agent, create an initial draft on disk (e.g. `docs/prd/task-management-app.md`), open the file, and ask a focused follow-up question to continue refining the document naturally.

---

## Core Concepts

### AI Agents

Six specialized agents cover the full documentation lifecycle:

| Agent | Phase | Purpose |
|-------|-------|---------|
| **PRD Creator** | PRD | Initial product concept and PRD generation |
| **Brainstormer** | PRD | Ideation and concept expansion |
| **Requirements Gatherer** | Requirements | Systematic requirements collection (EARS format) |
| **Solution Architect** | Design | Technical architecture and system design |
| **Specification Writer** | Implementation | Detailed technical specifications |
| **Quality Reviewer** | Implementation | Document validation and quality assurance |

### Workflow Phases

1. **PRD Phase** — Product concept and strategic goals
2. **Requirements Phase** — Detailed business and functional requirements
3. **Design Phase** — Technical architecture and solution decisions
4. **Implementation Phase** — Specifications, tasks, and quality review

### Document Folders

By default, documents are organized under:

```
docs/
  prd/          <- PRD Creator, Brainstormer
  requirements/ <- Requirements Gatherer
  design/       <- Solution Architect
  spec/         <- Specification Writer
  ideas/        <- Brainstorming sessions
```

---

## Usage Guide

### Natural-Language Sessions (Recommended)

Start a full iterative workflow with plain text — no slash command needed:

```
@docu I need to plan a REST API for a mobile banking app
```

Docu will:
1. Classify the request and assign the appropriate agent
2. Create an initial draft file on disk
3. Open the file in the editor
4. Ask one focused follow-up question per turn
5. Update the file with each response

Close the session with `done`, `finish`, or `/done`.

### Slash Commands

#### Create Documents
```
# New document with default template
@docu /new "Document Title"

# With a specific template
@docu /new "API Docs" --template basic

# With a custom path
@docu /new "User Guide" --path docs/guides/user-guide.md
```

#### Manage Agents
```
# List all available agents
@docu /agent list

# Switch to a specific agent
@docu /agent set requirements-gatherer

# Show the active agent
@docu /agent current
```

#### Manage Templates
```
# List all templates
@docu /templates list

# Show template details
@docu /templates show prd

# Validate a template
@docu /templates validate my-template
```

#### Update Documents
```
# Update a specific section
@docu /update --file docs/requirements.md --section "Scope" "Updated scope text"

# Append to a section
@docu /update --file docs/api.md --section "Authentication" --mode append "New auth notes"
```

#### Review Documents
```
# Standard review
@docu /review --file docs/requirements.md

# Strict review with automatic fixes
@docu /review --file docs/design.md --level strict --fix
```

### Full Workflow Example

```
# 1. Start with a PRD
@docu /agent set prd-creator
@docu /new "Mobile App PRD" --template prd

# 2. Brainstorm ideas
@docu /agent set brainstormer

# 3. Gather requirements
@docu /agent set requirements-gatherer
@docu /new "Mobile App Requirements" --template requirements

# 4. Design the solution
@docu /agent set solution-architect
@docu /new "Mobile App Architecture"

# 5. Write specifications
@docu /agent set specification-writer
@docu /new "Mobile App Tasks"

# 6. Quality review
@docu /agent set quality-reviewer
@docu /review --file docs/requirements/mobile-app-requirements.md --level strict
```

---

## Configuration

Configure Docu via VS Code Settings (`Ctrl+,` -> search "docu"):

| Setting | Default | Description |
|---------|---------|-------------|
| `docu.defaultDirectory` | `docs` | Default directory for new documents |
| `docu.defaultAgent` | `prd-creator` | Default agent on startup |
| `docu.templateDirectory` | `.vscode/docu/templates` | Custom templates location |
| `docu.autoSaveDocuments` | `true` | Auto-save created/updated documents |
| `docu.showWorkflowProgress` | `true` | Show phase transitions in chat |
| `docu.logging.level` | `info` | Log level (debug/info/warn/error/none) |
| `docu.telemetry.enabled` | `true` | Enable anonymous telemetry |
| `docu.debug.autoStart` | `false` | Auto-start local debug HTTP server |

### Custom Templates

Create YAML-frontmatter templates in `.vscode/docu/templates/`:

```yaml
---
id: my-template
name: My Custom Template
description: Template for my use case
variables:
  - name: title
    description: Document title
    required: true
    type: string
  - name: author
    required: false
    type: string
    defaultValue: Unknown
agentRestrictions:
  - requirements-gatherer
---

# {{title}}

**Author:** {{author}}
**Created:** {{currentDate}}

## Overview

{{overview}}
```

---

## Development

### Setup

```bash
git clone https://github.com/rodhayl/specItGithubCopilot.git
cd specItGithubCopilot
npm install
npm run compile
```

Press **F5** in VS Code to launch the Extension Development Host.

### Repository Structure

```text
specItGithubCopilot/
|- src/             # TypeScript source code
|  |- agents/       # AI agent implementations (6 agents)
|  |- commands/     # Slash command handlers
|  |- config/       # Configuration management
|  |- conversation/ # Conversation state and flow
|  |- debugging/    # Debug server (localhost only)
|  |- llm/          # Language model integration
|  |- templates/    # Template engine
|  `- tools/        # Tool implementations
|- tests/           # Jest test suite (unit + integration + e2e)
|- docs/            # Documentation
|- examples/        # Example projects and workflows
`- scripts/         # Build helper scripts
```

### Scripts

```bash
npm run compile        # TypeScript compilation
npm test               # Run test suite
npm run test:coverage  # Coverage report
npm run lint           # Type checking
npm run package        # Build VSIX
```

### Testing

See [docs/testing.md](docs/testing.md) for the full testing guide and [MANUAL_TEST.md](MANUAL_TEST.md) for manual verification steps.

---

## Advanced Features

### Security

- **Workspace Isolation** — All file operations restricted to the current workspace
- **Path Validation** — Prevents directory traversal attacks
- **Input Sanitization** — Cleans potentially malicious content from all inputs
- **Data Anonymization** — Telemetry data anonymized by default

### Offline Mode

When GitHub Copilot is unavailable, Docu provides:
- Basic file operations and template processing
- Document structure management
- Automatic detection and graceful fallback

### Diagnostics

Via the Command Palette:
- `Docu: Show Diagnostics` — System diagnostics panel
- `Docu: Export Diagnostics` — Export diagnostic report as JSON
- `Docu: Show Output Channel` — Extension logs
- `Docu: Toggle Debug Mode` — Enable/disable verbose logging

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

[MIT](LICENSE.md) — Copyright (c) 2026 rodhayl
