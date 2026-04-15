# User Context: ravi.poli@cloudfuze.com

> Derived from Claude conversation history via ccmigrate.
> Source: 14 conversations

## Work Context from Conversations

### Top ways companies use clay

Here are the top 3 ways companies are using **Clay (clay.com)** — the AI-powered data enrichment and outbound sales platform:

**1. Automated Lead Enrichment & Prospecting**
Sales and growth teams use Clay to automatically pull data from 75+ data sources (LinkedIn, Clearbit, Apollo, etc.) to build hyper-detailed prospect lists. Instead of manually researching leads, companies enrich contacts with job titles, tech stacks, funding rounds, and more — all in one place.

**2. Hyper-Personalized Outbound Campaigns**
Clay's AI writing feature (powered by GPT) lets companies craft highly personalized cold emails at scale. By combining enriched data with AI, teams can reference a prospect's recent LinkedIn post, company news, or tech stack in every email automatically — dramatically improving reply

### Claude's Java compatibility

Here's a detailed guide for deploying to multiple bare metal servers using Bitbucket Pipelines and Claude Code:

---

## Architecture Overview

```
Bitbucket Pipelines
        ↓
   Build JAR file
        ↓
Deploy script SSHes into each server
   /         |         \
Server1    Server2    Server3
   \         |         /
      Load Balancer
      (NGINX/HAProxy)
          ↓
        Users
```

---

## Step 1 — Set Up SSH Access from Pipelines to Servers

On each bare metal server, add the Bitbucket Pipeline's public key:
```bash
# On each server
mkdir -p ~/.ssh
echo "your-pipeline-public-key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

In **Bitbucket → Repository Settings → Repository Variables**, add:
```
SSH_PRIVATE_KEY   = (your private key)
SERVER1_IP        = 192.16

### HubSpot assistance

Let me gather more details on Lawrence's deals to give specific, actionable recommendations.
```
This block is not supported on your current device yet.
```

Here are Lawrence's top 10 specific, actionable recommendations based on his deal data:

---

**1. Revive Shivaami Cloud Services — $100K at risk of being lost**
This is Lawrence's biggest deal ($100K) but the last contact was back in **November 2023** — over 14 months ago — and the close date was April 2024. It's sitting in "Unresponsive" stage. This deal alone represents 25% of his open pipeline. Lawrence should make immediate contact, re-qualify the opportunity, and either move it forward or close it out to keep his pipeline clean.

**2. Urgently push Integris across the finish line — $60K closing March 31**
Integris ($60K) has a c

### Decision-making frameworks analysis

```
This block is not supported on your current device yet.
```

Before diving in, let me ask one key question to make sure this is as useful as possible:
```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```

Let me ask you a couple of quick questions before building this out:

**1. What's your primary goal with this analysis?**
- Learn the major frameworks (overview/education)
- Apply a framework to a specific decision I'm facing
- Compare frameworks to choose the right one for my work
- Build a reference tool for my team

**2. What context are you working in?**
- Personal/life decisions
- Business/strategy
- Product/design
- Research/academia

While you answer, I'll go ahead and build a comprehensive interactive

### SaaS agent platform MVP planning and deployment strategy

Sure — let me walk through the complete workflow system, from how a user triggers a workflow through chat, to how it executes on the backend, to how progress shows up in real-time in the UI.

---

## The Big Picture

There are three parts to the workflow system:

1. **Workflow Definitions** — templates stored in the database that map roles to apps and define the steps
2. **Workflow Execution Engine** — BullMQ (or Trigger.dev) running the actual steps on the backend
3. **Chat Interface** — how users trigger, monitor, and configure workflows through the agent

Let me go through each one.

---

## Part 1: Backend — Workflow Execution

### Option A: BullMQ (from the ADR recommendation)

With BullMQ, each workflow is a **job processor** — a TypeScript function registered with a queue. When the 