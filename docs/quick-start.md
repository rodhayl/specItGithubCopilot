# Quick Start Guide

Get up and running with Docu in 5 minutes! This guide covers the essentials to start creating AI-powered documentation.

## 🚀 5-Minute Setup

### Step 1: Prerequisites (2 minutes)

**Required:**
- ✅ VS Code 1.97.0+ installed
- ✅ GitHub Copilot subscription active
- ✅ GitHub Copilot extension installed

**Quick Check:**
1. Open VS Code
2. Press `Ctrl+Shift+I` (or `Cmd+Shift+I` on Mac)
3. GitHub Copilot Chat should open

### Step 2: Install Docu (1 minute)

**Option A: From Marketplace** (Coming Soon)
- Extensions → Search "Docu" → Install

**Option B: From VSIX**
- Download VSIX file
- `Ctrl+Shift+P` → "Extensions: Install from VSIX"
- Select the VSIX file

### Step 3: Verify Installation (1 minute)

1. Open Copilot Chat (`Ctrl+Shift+I`)
2. Type: `@docu`
3. You should see Docu respond!

### Step 4: Create Your First Document (1 minute)

```
@docu /new "My First Document"
```

**Success!** You should see:
- ✅ New document created
- ✅ File opened in VS Code
- ✅ Clickable link in chat

## 🎯 Essential Commands

### Document Creation
```bash
# Basic document
@docu /new "Document Title"

# With template
@docu /new "Product Requirements" --template prd

# In specific folder
@docu /new "User Guide" --path docs/
```

### Agent Management
```bash
# List agents
@docu /agent list

# Switch agent
@docu /agent set prd-creator

# Current agent
@docu /agent current
```

### Templates
```bash
# List templates
@docu /templates list

# Show template details
@docu /templates show prd

# Create custom template
@docu /templates create my-template --interactive
```

### Document Updates
```bash
# Update section
@docu /update --file README.md --section "Installation" "New content"

# Review document
@docu /review --file requirements.md --level normal
```

## 🤖 Meet the Agents

| Agent | Use When | Example |
|-------|----------|---------|
| **PRD Creator** 🎯 | Starting new products/features | `@docu /agent set prd-creator` |
| **Brainstormer** 💡 | Exploring ideas and possibilities | `@docu /agent set brainstormer` |
| **Requirements Gatherer** 📋 | Defining detailed requirements | `@docu /agent set requirements-gatherer` |
| **Solution Architect** 🏗️ | Designing technical solutions | `@docu /agent set solution-architect` |
| **Specification Writer** 📝 | Planning implementation | `@docu /agent set specification-writer` |
| **Quality Reviewer** 🔍 | Reviewing and improving docs | `@docu /agent set quality-reviewer` |

## 📝 Quick Workflow Example

### Create a Feature Documentation Set (10 minutes)

**1. Product Requirements (2 minutes)**
```bash
@docu /agent set prd-creator
@docu /new "User Login Feature PRD" --template prd
```
*Chat with agent about business goals, users, success metrics*

**2. Detailed Requirements (3 minutes)**
```bash
@docu /agent set requirements-gatherer
@docu /new "User Login Requirements" --template requirements
```
*Define user stories and acceptance criteria*

**3. Technical Design (3 minutes)**
```bash
@docu /agent set solution-architect
@docu /new "User Login Architecture" --template basic
```
*Discuss technical approach, security, APIs*

**4. Implementation Plan (2 minutes)**
```bash
@docu /agent set specification-writer
@docu /new "User Login Implementation" --template basic
```
*Break down into development tasks*

**Result:** Complete documentation set ready for development!

## ⚙️ Quick Configuration

### Essential Settings

Add to your VS Code settings (`Ctrl+,` → search "docu"):

```json
{
  "docu.defaultDirectory": "docs",
  "docu.defaultAgent": "prd-creator",
  "docu.autoSaveDocuments": true,
  "docu.logging.level": "info"
}
```

### Workspace Setup

Create `.vscode/settings.json` in your project:

```json
{
  "docu.defaultDirectory": "documentation",
  "docu.templateDirectory": ".vscode/docu/templates"
}
```

## 🛠️ Common Use Cases

### API Documentation
```bash
@docu /agent set solution-architect
@docu /new "API Documentation" --template basic
# Discuss endpoints, authentication, examples
```

### Meeting Notes
```bash
@docu /templates create meeting-notes --interactive
@docu /new "Team Meeting Notes" --template meeting-notes
```

### Bug Reports
```bash
@docu /new "Bug Report Template" --template basic
# Create reusable bug report structure
```

### User Guides
```bash
@docu /agent set specification-writer
@docu /new "User Guide" --template basic
# Step-by-step instructions and tutorials
```

## 🔧 Troubleshooting

### Not Working?

**Problem:** `@docu` doesn't respond
**Quick Fix:**
1. Check GitHub Copilot is signed in
2. Restart VS Code
3. Try: `@docu /help`

**Problem:** Commands not recognized
**Quick Fix:**
1. Always use `@docu` prefix
2. Check command spelling
3. Try: `@docu /agent list`

**Problem:** Can't create files
**Quick Fix:**
1. Open a workspace folder
2. Check file permissions
3. Try different directory

### Need Help?

- **Quick Help:** `@docu /help`
- **Diagnostics:** `@docu /diagnostics`
- **Full Guide:** [Complete Tutorial](complete-tutorial.md)
- **FAQ:** [Frequently Asked Questions](faq.md)

## 🎓 Next Steps

### Learn More
- 📖 [Complete Tutorial](complete-tutorial.md) - Comprehensive guide
- 🔍 [Command Reference](command-reference.md) - All commands
- 🤖 [Agent Guide](agents.md) - Detailed agent information

### Explore Examples
- 🏠 [Smart Home Demo](../examples/demo-project/) - Complete project example
- 🔄 [Workflow Examples](../examples/workflows/) - Real-world scenarios
- 📋 [Template Examples](../examples/templates/) - Custom templates

### Advanced Features
- 📊 Multi-document operations
- 🔄 Automated quality reviews
- 🎨 Custom template creation
- ⚡ Workflow automation

---

**Ready to create amazing documentation?** Start with `@docu /new "My Project Documentation"` and let the AI guide you! 🚀

**Questions?** Check the [FAQ](faq.md) or [open a discussion](https://github.com/rodhayl/specItGithubCopilot/discussions).