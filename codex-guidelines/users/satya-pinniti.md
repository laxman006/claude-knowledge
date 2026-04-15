# User Context: satya.pinniti@cloudfuze.com

> Derived from Claude conversation history via ccmigrate.
> Source: 3 conversations

## Work Context from Conversations

### Using Claude code

Claude Code is a command-line tool that lets you use Claude as an AI coding assistant directly in your terminal. Here's how to get started:

## Installation

You need Node.js 18+ installed, then run:

```bash
npm install -g @anthropic-ai/claude-code
```

## Setup

Authenticate by running:

```bash
claude
```

It will prompt you to log in with your Anthropic account (or API key).

## Basic Usage

**Start an interactive session:**
```bash
claude
```

**Ask a one-off question:**
```bash
claude "explain this codebase"
```

**Run with a specific file in context:**
```bash
claude --file myfile.py "refactor this function"
```

## What You Can Do With It

- **Understand code** – Ask Claude to explain files, functions, or entire repos
- **Write & edit code** – Generate new code or modify existing f

### Claude code vs interface code differences

Claude Code and the code features in this interface (claude.ai) are different products, though both are powered by Claude AI models.

**Claude Code** is a standalone command-line tool (CLI) you install on your computer. It's designed for agentic coding tasks — it can read and edit files on your local machine, run terminal commands, work with your git repositories, and operate directly in your development environment.

**The code features here on claude.ai** are built into the chat interface. You can ask Claude to write, explain, or debug code, and Claude can also create interactive Artifacts (like runnable React components or HTML pages) that render right in the browser.

The key differences:

- **Claude Code** runs locally on your machine and can directly touch your files and codebase
- *