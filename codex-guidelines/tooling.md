# Tooling

> Derived from Claude conversation history via ccmigrate.

## Comprehensive app development prompt with exceeds expectations rubric

```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```


```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```


```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```


```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```


```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```


```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

There are your six phases, each self-contained with enough context that Claude Code won't need to reference the others. A few tips on using them:

**Wait for each phase to fully compile and run before moving to the next.** If you paste Phase 2 while Phase 1 has broken navigation, the errors will cascade.

**Copy-paste each phase as-is** — I've included the context reminder at the top of every prompt so Claude Code knows what's already been built without you needing to re-paste earlier phases.

**Phase 3 is the biggest one.** If Claude Code struggles with it, you can split it further by giving it one screen at a time (just copy the relevant sub-section like "3A. Home / Dashboard" with the context paragraph).

**Phase 6 is the one most teams skip** and it's worth 5 rubric points on its own plus it strengthens your presentation delivery scores. Don't leave it for the last minute.

## Comprehensive app development prompt with exceeds expectations rubric

```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

Phase 2 now includes the full Firebase iOS setup — project config, `app.json` settings with all the required iOS permissions, Firestore security rules, EAS Build steps for competition day, and the backup video plan. When you paste this into Claude Code, it'll handle both the Firebase integration and the onboarding flow in one go.

## HubSpot CRM analysis: Q4 2025 and Q1 2026 revenue performance

The user is asking me to analyze HubSpot CRM data comprehensively. This is a detailed request that requires:

1. Revenue Performance analysis
2. Manager Forecast Accuracy
3. MQL Assignment Analysis
4. Pipeline Stage & Deal Velocity
5. Q1 2026 Revenue Projection
6. Key Insights

All broken down by team: SMB, AM (Mid-Market), and Enterprise.

To do this, I need to:
1. First search for and load HubSpot tools
2. Query HubSpot for CRM data including:
   - Deals (with stage, close date, amount)
   - Contacts/Companies 
   - MQLs
   - Pipeline data
3. Create a comprehensive analysis artifact

Let me start by using tool_search to find the right HubSpot tools for this analysis.I'll help you analyze HubSpot CRM data comprehensively. Let me first load the HubSpot tools to access your CRM data.
```
This block is not supported on your current device yet.
```

Good! The HubSpot tools are now loaded. Now I need to start by getting user details to understand what access we have, and then query for:

1. Deals data - closed won deals for Q4 2025 and Q1 2026
2. Pipeline data - forecast values, stages, etc.
3. MQL data - lead generation and assignment
4. Deal stages and timing

Let me start with get_user_details to understand the account and then query for deals.Now let me access your HubSpot account to gather the CRM data:
```
This block is not supported on your current device yet.
```

Great! I have access to DEAL, CONTACT, and COMPANY objects for both read and write. Now I need to query for:

1. Deals closed in Q4 2025 (Oct 1 - Dec 31, 2025)
2. Deals closed in Q1 2026 (Jan 1 - Mar 31, 2026)
3. Pipeline data with forecast values
4. MQL data

Let me start by searching for deals with the relevant properties. I need to understand what properties are available for deals first, then query for closed won deals in Q4 2025 and current progress in Q1 2026.

Let me search for closed-won deals with relevant properties for revenue analysis, including:
- Deal amount
- Deal stage
- Close date
- Te

## Completing a project or task

The user is asking about what they (as a CloudFuze partner/representative) can do from the CloudFuze side to manually activate/complete the subscription if the client has done their part but the status is still stuck.## What YOU Can Do From CloudFuze Side

---

### 🔑 CloudFuze Has a Partner/Admin Dashboard

As a CloudFuze partner, you should have access to **CloudFuze's Admin Portal**. Here's what to check:

---

### Step 1 - Login to CloudFuze Admin Panel
- Login to your **CloudFuze Partner Dashboard**
- Look for a section called:
  - **"Subscriptions"** or
  - **"Microsoft Marketplace Orders"** or
  - **"Pending Activations"**

---

### Step 2 - Find Estee Lauder's Subscription
- Search for the customer **Estee Lauder**
- Or search using Subscription ID:
  `ca296772-6c6a-4de9-dbf1-999ed07a33f4`

---

### Step 3 - Manually Activate It
- You should see a button like:
  - **"Activate Subscription"**
  - **"Fulfill Order"**
  - **"Mark as Active"**
- Click it to **manually trigger the activation**

---

### Step 4 - Confirm via Microsoft API
CloudFuze needs to send a **confirmation back to Microsoft** by calling:
> `POST /api/saas/subscriptions/{subscriptionId}/activate`

This tells Microsoft — *"Yes, we have fulfilled this order"*

**If you are technical**, you can check with CloudFuze's tech team if this API call was made or not

---

### 🚨 Most Likely Root Cause
| Problem | What Happened |
|---------|--------------|
| **Webhook not received** | CloudFuze never got notified by Microsoft |
| **Webhook received but not processed** | CloudFuze got the notification but ignored it |
| **Activation API not called** | CloudFuze forgot to confirm back to Microsoft |

---

### ✅ Simple Summary

1. Login to **CloudFuze Admin Panel**
2. Find **Estee Lauder's order**
3. **Manually activate** it from there
4. Wait **1-2 hours**
5. Check Partner Center again — status should change to ✅ **Active**

> 💡 **Key Point:** The fix is almost always on **CloudFuze's side** — they need to 

## SaaS agent platform MVP planning and deployment strategy

Got it — let me just give you the answer directly here in chat.

---

## 1. Agent Architecture

### The Core Loop (No LangGraph Needed)

Your agent is a simple TypeScript function that runs a **ReAct loop** using Claude API's native tool-use. No frameworks. About 150-200 lines of code. Here's how it works:

**Step 1:** User sends a message via the chat UI ("Who has unused Figma licenses?")

**Step 2:** Your backend sends that message to Claude API along with a system prompt (containing org context) and a list of tool definitions (JSON schemas describing what the agent can do)

**Step 3:** Claude responds with either:
- A **text response** (final answer) → stream it to the user, done
- A **tool_use block** (e.g., `query_licenses` with params `{app: "Figma", status: "inactive"}`) → execute the tool server-side, send the result back to Claude as a `tool_result`, and loop back to Step 3

**Step 4:** Claude sees the tool result, reasons about it, and either calls another tool or gives the final text answer. The loop continues until Claude produces a final text response (or you hit a max iteration limit, say 10 loops).

That's it. No state machines, no graph frameworks. Just a while loop.

### Tool Definitions (MVP — 8 Tools)

The agent needs these tools for the MVP:

**Read-only tools (no approval needed):**
- `query_apps` — list discovered apps, filter by status (approved/shadow/pending), category
- `query_users` — list users, filter by department, role, app assignments
- `check_licenses` — get license data for an app: total seats, used seats, inactive users
- `get_usage_stats` — usage scores per user per app, last active dates
- `get_spend_summary` — aggregate spend by app, department, time period
- `check_workflow_status` — look up a running or completed workflow execution

**Action tools (require confirmation):**
- `trigger_workflow` — start an onboarding/offboarding/reclamation job via BullMQ. The agent should ALWAYS confirm with the user before calling this for des
