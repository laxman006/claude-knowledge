# Code Review

> Derived from Claude conversation history via ccmigrate.

## Understanding and implementing gstack for your team

The user's CEO sent them information about "gstack" - a Claude Code setup by Garry Tan (YC President). Let me explain everything clearly and step by step.

Let me break down what this is and how to implement it.

Key facts:
- gstack is a collection of slash commands for Claude Code
- GitHub repo: https://github.com/garrytan/gstack
- It gives Claude Code specialized "modes" or "cognitive gears"
- 8 slash commands (the post mentions 8, but the GitHub description says 6)

Let me explain this comprehensively for someone who may not be deeply technical.

The commands mentioned:
- /plan-ceo-review - founder mode, strategic thinking


- /browse - visual QA with browser
- /qa - git diff analysis, health score

I'll give a thorough explanation covering: what it is, why it matters, how to set it up, and how to use it for their team.# Complete Guide to gstack — Garry Tan's Claude Code Setup

Let me break this down completely — what it is, why your CEO wants it, and exactly how to set it up.

---

## 🧠 What Is This, Simply Put?

**Claude Code** is Anthropic's AI coding tool that runs in your terminal. It can read your codebase, write code, fix bugs, review PRs — basically a senior engineer sitting next to you.

**The problem?** Claude Code has one "brain mode" for everything. Whether you're planning a feature or shipping a release, it thinks the same way.

**gstack solves this** by giving Claude Code **specialized roles** — like switching hats:

| Slash Command | Role It Plays |
|---|---|
| `/plan-ceo-review` | Acts like a Founder/CEO — asks "is this the right thing to build?" |
| `/browse` | Acts like a QA tester — opens a browser, takes screenshots, finds bugs |
| `/qa` | Acts like a Release Manager — checks your code changes, scores them /100 |
| + more | Eng Manager, PR Reviewer, etc. |

Think of it like this: instead of one generalist assistant, you now have **a team of specialists** you can summon on demand.

---

## 🎯 Why Does Your CEO Want This?

Your CEO is saying: **"
