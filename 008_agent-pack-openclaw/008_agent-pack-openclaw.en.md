---
# Unique identifier linking EN/ES versions
content_id: "lead-magnets-agent-pack-openclaw"

# Locale (must match filename)
locale: "en"

# SEO & Display
title: "AI Agent Pack for OpenClaw"
description: "Ready-to-use agent pack: a personal assistant specialized in Notion, configured for OpenClaw. Includes identity, operations, Notion skill and CLI script."

# Category and taxonomy
category: "ai-agents"
tags:
  - openclaw
  - ai-agents
  - notion
  - automation
  - personal-assistant

# Metadata
difficulty: "beginner"
version: "1.0.0"
published: true
order: 8
lastUpdated: "2026-03-03"
author: "AIPaths Academy"
downloadSize: "15 KB"
estimatedSetupTime: "10 minutes"

# Prerequisites
prerequisites:
  - OpenClaw installed
  - Notion account with an integration created
  - Notion API key
  - Node.js installed

# Files included
files:
  - path: pack/SOUL.md
    description: Agent identity — who it is, how it behaves, core values
  - path: pack/AGENTS.md
    description: Operations — capabilities, platforms, behavior rules
  - path: pack/skills/notion-juan/SKILL.md
    description: Notion skill with available commands
  - path: pack/skills/notion-juan/scripts/notion.mjs
    description: CLI script to interact with the Notion API
  - path: pack/README.md
    description: Installation guide and pack structure
---

# AI Agent Pack for OpenClaw

Your first AI agent, ready to install. "Juan" is a personal assistant specialized in Notion that manages tasks, structures projects, and keeps everything organized.

## What's Included

- **SOUL.md** — The agent's identity: who it is, its personality, how it communicates
- **AGENTS.md** — Operations: what it can do, capabilities, behavior rules
- **Notion Skill** — Complete skill with CLI script to create pages, databases, search content and more
- **README.md** — Step-by-step installation guide

## Why This Pack

Most people install OpenClaw and don't know what to do next. This pack gives you a working agent in 10 minutes so you can understand how the `.md` file system, skills, and scripts work.

## Quick Setup

1. **Download and unzip** the pack
2. **Copy the files** to your agent directory in OpenClaw
3. **Set your Notion API key** as an environment variable
4. **Start the agent** and begin using it

## What You'll Learn

- How an agent is structured with SOUL.md and AGENTS.md
- How skills and scripts work
- How to connect an agent to an external API (Notion)
- The foundations for creating your own custom agents

## Related Resources

- [Video: My 6 AI Agents on a Mac Mini](https://www.youtube.com/@aipaths)
- [Waitlist: Complete agents course](https://www.aipaths.academy/es/openclaw-course)
