# Coding Guidelines and Standards

> Derived from Claude conversation history via ccmigrate.
> Review before applying to Codex workspace.

## Claude's Java compatibility

Great question! Here's how deployment and Bitbucket integration typically work with Claude Code:

---

## Bitbucket Integration

Claude Code works with any Git-based repository, including Bitbucket. You interact with it through your **local Git setup**:

**Basic Workflow:**
1. Clone your Bitbucket repo locally as usual
2. Run Claude Code in that directory
3. Claude Code reads, edits, and creates files in your project
4. You then use standard Git commands to commit and push

```bash
git add .
git commit -m "Fix: resolved null pointer exception in UserService"
git push origin feature/my-fix
```

Claude Code can also **suggest or run Git commands** for you during a session, like creating branches or staging files.

---

## Deployment After Code Fixes

Deployment depends on your existing pipeline, but Claude Code can help with each stage:

**Option 1 – CI/CD via Bitbucket Pipelines**
- Claude Code can help you write or fix your `bitbucket-pipelines.yml` file
- Once you push your changes, Bitbucket Pipelines picks up automatically and runs your build, test, and deploy steps

**Option 2 – Manual Deployment**
- Claude Code can run build commands directly (e.g., `mvn package`, `gradle build`)
- You then deploy the resulting artifact manually to your server or cloud

**Option 3 – Cloud Platforms**
Claude Code can assist with deployment configs for platforms like:
- AWS (Elastic Beanstalk, ECS, Lambda)
- GCP (Cloud Run, App Engine)
- Azure (App Service)
- Kubernetes (generating/editing YAML manifests)
- Docker (writing Dockerfiles and Compose files)

---

## Typical End-to-End Flow

```
Claude Code fixes Java code
        ↓
git commit & push to Bitbucket
        ↓
Bitbucket Pipelines triggers
        ↓
Build (Maven/Gradle) → Test (JUnit) → Deploy (your target)
```

---

## What Claude Code Can Help You Set Up

- A `bitbucket-pipelines.yml` tailored to your Java/Maven/Gradle project
- Dockerfile for containerizing your Java app
- Deployment scripts for your target environment
- Environment variable and secrets management guidance

Would you like help setting up a specific part of this — like a Bitbucket Pipelines config for a Spring Boot app, or a Dockerfile for your Java project?

## Claude analysis of HubSpot data

# CF Manage — Product Requirements Documents
## NFC Legal | 4 Feature PRDs | March 2026

---

# PRD 1: HR Form-Driven License Provisioning

**Feature Name:** HR Form Input Trigger for License Provisioning
**Priority:** High
**Requested By:** NFC Legal
**Target Delivery:** 30 days from contract signing

---

### 1. Objective

Enable CF Manage workflows to accept an HR form submission as a trigger input, parse the requested licenses and applications from the form, and automatically provision them to the specified user — without any manual IT intervention.

---

### 2. Background & Problem Statement

Currently, NFC Legal's HR team submits license and software access requests through a form-based process. These requests are then manually interpreted by IT and actioned one by one. This creates delays, dependency on IT availability, and risk of human error. The goal is to close the loop between HR's existing form workflow and CF Manage's provisioning engine so that form submission directly initiates automated provisioning.

---

### 3. Scope

**In Scope**
- Accepting a form payload (from tools such as Microsoft Forms, Google Forms, Typeform, or a custom HR system) as a workflow trigger
- Parsing form fields to identify the user, requested licenses, and applications
- Mapping form line items to CF Manage provisioning actions
- Executing provisioning automatically upon form submission
- Sending a confirmation notification to IT and HR upon completion

**Out of Scope**
- Building or modifying the HR form itself
- Managing form authentication or HR system login
- Provisioning hardware or physical assets

---

### 4. User Stories

- As an **HR manager**, I want to submit a form requesting licenses for a new or existing user so that provisioning happens automatically without emailing IT.
- As an **IT admin**, I want form submissions to trigger provisioning workflows in CF Manage so that I don't have to manually interpret and action each request.
- As a **CF Manage workflow engine**, I want to receive structured form data, parse the requested items, and map them to provisioning tasks so that the correct licenses and apps are assigned to the correct user.

---

### 5. Functional Requirements

| ID | Requirement |
|----|-------------|
| F1 | The system shall support receiving form submissions via webhook or API callback |
| F2 | The system shall parse key fields: requester name, target user, list of requested licenses/apps, and urgency flag |
| F3 | The system shall map each parsed line item to a corresponding CF Manage provisioning action |
| F4 | The system shall execute provisioning actions in sequence upon successful form parsing |
| F5 | The system shall send a confirmation email to the HR submitter and IT admin upon completion |
| F6 | The system shall log all form-triggered provisioning events with timestamp, form data snapshot, and outcome |
| F7 | If a line item cannot be mapped to a known provisioning action, the system shall flag it and notify IT for manual review |
| F8 | The system shall support at minimum: Microsoft Forms, Google Forms, and generic webhook payloads |

---

### 6. Non-Functional Requirements

- **Reliability:** Form-to-provisioning execution should complete within 5 minutes of form submission under normal load
- **Security:** Form payloads must be received over HTTPS; sensitive fields must not be logged in plain text
- **Auditability:** Every provisioning action triggered by a form must be traceable back to the originating form submission
- **Scalability:** Must support concurrent form submissions without queuing delays

---

### 7. Workflow Diagram (Described)

1. HR submits form → 2. Webhook fires to CF Manage → 3. Parser extracts user + requested items → 4. Items mapped to provisioning actions → 5. Workflow executes provisioning → 6. Confirmation sent to HR + IT → 7. Event logged in audit trail

---

### 8. Acceptance Criteria

- A form submitted with three license requests provisions all three to the correct user within 5 minutes
- An unrecognized line item triggers an IT notification rather than failing silently
- All provisioning events appear in the CF Manage audit log with form metadata attached
- Confirmation emails are received by both HR and IT upon completion

---

### 9. Dependencies

- Webhook endpoint must be exposed and secured in CF Manage infrastructure
- Form field naming conventions must be agreed upon with NFC Legal's HR team during onboarding
- Microsoft 365 API access must be active for license assignment execution

---

### 10. Open Questions

- Which HR form tool is NFC Legal currently using?
- Are there approval steps required before provisioning, or should form submission trigger provisioning directly?
- Should partial provisioning (some items succeed, some fail) trigger a rollback or proceed with notification?

---
---

# PRD 2: Dynamic Group Assignment During Onboarding

**Feature Name:** Conditional Group Assignment Prompt in Onboarding Workflow
**Priority:** High
**Requested By:** NFC Legal
**Target Delivery:** 30 days from contract signing

---

### 1. Objective

Extend the CF Manage onboarding workflow to include a dynamic prompt that asks whether the new user should be added to any groups beyond their standard role-based assignment, and automatically executes the group additions based on the response.

---

### 2. Background & Problem Statement

CF Manage currently assigns users to groups based on static role templates. However, individual users often need to be added to project-specific groups, distribution lists, or security groups that fall outside their role template. This currently requires a separate manual step by IT after onboarding is complete. The goal is to capture this requirement within the onboarding workflow itself, eliminating the follow-up action.

---

### 3. Scope

**In Scope**
- A conditional prompt step within the onboarding workflow asking about additional group membership
- A multi-select interface for choosing from available Microsoft 365 groups, distribution lists, and security groups
- Automatic execution of group additions based on selections made
- Support for "none" response that skips the step cleanly

**Out of Scope**
- Creating new groups from within the onboarding workflow
- Managing group permissions or ownership
- Syncing groups from third-party directory services other than Microsoft 365

---

### 4. User Stories

- As an **IT admin**, I want to be prompted during onboarding to select additional groups for the new user so that I don't have to return and manually add them after the fact.
- As a **hiring manager**, I want to specify additional group memberships during the onboarding request so that the new team member has access to the right collaboration spaces from day one.
- As a **CF Manage workflow**, I want to present available groups, capture selections, and execute additions so that group membership is complete by the end of the onboarding run.

---

### 5. Functional Requirements

| ID | Requirement |
|----|-------------|
| F1 | The onboarding workflow shall include a conditional step that prompts: "Should this user be added to any additional groups?" |
| F2 | The prompt shall present a searchable, multi-select list of available M365 groups, distribution lists, and security groups |
| F3 | The system shall support a "No additional groups" option that skips the step without error |
| F4 | Upon selection, the system shall automatically add the user to all selected groups via Microsoft Graph API |
| F5 | Group additions shall be completed before the onboarding workflow marks the run as complete |
| F6 | The system shall log all group additions with timestamp and the identity of who made the selection |
| F7 | If a group addition fails, the system shall notify IT and continue completing the rest of the onboarding workflow |
| F8 | The prompt may be presented to either the IT admin or a delegated requestor (e.g. hiring manager), configurable per workflow |

---

### 6. Non-Functional Requirements

- **Performance:** Group list must load within 3 seconds for directories with up to 500 groups
- **Usability:** Search and filter must be available to handle large group lists efficiently
- **Auditability:** All group additions must appear in both CF Manage logs and Microsoft 365 audit logs

---

### 7. Workflow Diagram (Described)

1. Onboarding workflow starts → 2. Role-based template applied → 3. Prompt: additional groups needed? → 4a. Yes: present group selector → user selects → groups added via Graph API → 4b. No: step skipped → 5. Onboarding marked complete → 6. Confirmation sent → 7. Event logged

---

### 8. Acceptance Criteria

- During an onboarding run, the additional group prompt appears after role-based template application
- Selecting two groups results in the user being added to both before onboarding completes
- Selecting "No additional groups" completes onboarding without error
- All group additions are visible in the CF Manage audit log

---

### 9. Dependencies

- Microsoft Graph API access with Group.ReadWrite.All permission
- Group directory must be synced or accessible in real time within CF Manage

---

### 10. Open Questions

- Should group selection be available to the hiring manager via a self-service form, or only to IT admins within the CF Manage UI?
- Should there be a maximum number of groups that can be added in a single onboarding run?
- Should previously used group combinations be saveable as a preset for future use?

---
---

# PRD 3: Email Delegation and Forwarding Rules on Offboarding

**Feature Name:** Mailbox Delegation and Forwarding Rule Configuration in Offboarding Workflow
**Priority:** High
**Requested By:** NFC Legal
**Target Delivery:** 30 days from contract signing

---

### 1. Objective

Integrate a mailbox continuity step into the CF Manage offboarding workflow that allows an administrator to choose between setting up mailbox delegation or configuring a forwarding rule for the departing user's email, and executes the selected configuration automatically.

---

### 2. Background & Problem Statement

When employees leave NFC Legal, their mailbox continues to receive business-critical communications. Without a delegation or forwarding rule in place, these emails go unread and can result in missed client communications, compliance issues, or loss of business continuity. Currently this is handled manually and inconsistently. The goal is to make mailbox continuity a required, automated step in every offboarding workflow.

---

### 3. Scope

**In Scope**
- A mailbox continuity step embedded in the offboarding workflow
- Option A: Grant delegate/full access to a specified mailbox
- Option B: Set up a forwarding rule to a specified email address
- Automatic execution of the selected configuration via Microsoft Graph API / Exchange Online
- Notification to the designated delegate or forwarding recipient confirming setup

**Out of Scope**
- Managing shared mailbox creation
- Setting litigation hold or legal archiving (separate compliance feature)
- Configuring auto-reply / out-of-office messages (can be a future addition)

---

### 4. User Stories

- As an **IT admin**, I want the offboarding workflow to prompt me to set up delegation or forwarding so that I never accidentally skip this step.
- As a **supervising partner at NFC Legal**, I want to receive delegated access or forwarded emails from a departing attorney's mailbox so that client matters are not disrupted.
- As a **CF Manage workflow**, I want to execute the mailbox continuity configuration automatically so that IT does not need to open Exchange Admin Center separately.

---

### 5. Functional Requirements

| ID | Requirement |
|----|-------------|
| F1 | The offboarding workflow shall include a mandatory mailbox continuity step before account deactivation |
| F2 | The step shall present two options: (a) Set up mailbox delegation, (b) Set up forwarding rule |
| F3 | For delegation: the system shall accept a target user input and grant Full Access and Send As permissions to that user |
| F4 | For forwarding: the system shall accept a target email address and configure a server-side forwarding rule on the departing user's mailbox |
| F5 | The system shall execute the selected configuration before proceeding to account deactivation and license removal |
| F6 | The designated delegate or forwarding recipient shall receive an automated notification confirming the setup |
| F7 | A "Skip" option shall be available for IT admins with appropriate permissions, with a mandatory reason field |
| F8 | All mailbox continuity actions shall be logged with the selected option, target address, timestamp, and admin identity |

---

### 6. Non-Functional Requirements

- **Reliability:** Mailbox continuity configuration must complete before account deactivation is triggered — these steps must be sequenced, not parallelized
- **Security:** Delegation and forwarding rules must only be configurable by authorized CF Manage admins
- **Auditability:** All configurations must be logged in CF Manage and traceable in Exchange audit logs

---

### 7. Workflow Diagram (Described)

1. Offboarding triggered → 2. Mailbox continuity prompt presented → 3a. Delegation selected: target user entered → Full Access + Send As granted → 3b. Forwarding selected: target address entered → server-side rule created → 4. Notification sent to recipient → 5. Account deactivation proceeds → 6. License revocation → 7. Event logged

---

### 8. Acceptance Criteria

- The offboarding workflow does not proceed to account deactivation until the mailbox continuity step is completed or explicitly skipped
- Selecting delegation results in the target user having Full Access to the departing user's mailbox before account deactivation
- Selecting forwarding results in all incoming mail being forwarded to the specified address
- The recipient receives a notification email confirming their access or forwarding setup
- The skip action requires a reason and is logged

---

### 9. Dependencies

- Exchange Online / Microsoft Graph API access with MailboxSettings.ReadWrite and FullAccess delegation permissions
- Offboarding workflow must be sequenced so mailbox step precedes account deactivation

---

### 10. Open Questions

- Should both delegation and forwarding be configurable simultaneously, or is it one or the other?
- What is NFC Legal's standard policy for how long delegation/forwarding should remain active after offboarding?
- Should the system automatically remove delegation or forwarding after a set period (e.g. 90 days)?

---
---

# PRD 4: Application Access Request Portal

**Feature Name:** Self-Service Application Access Request Portal with Approval Workflow and Credential Delivery
**Priority:** High
**Requested By:** NFC Legal
**Target Delivery:** 30 days from contract signing

---

### 1. Objective

Build a self-service portal within CF Manage where users can browse available applications, submit access requests, route those requests through a configurable approval workflow, and upon approval automatically receive an email with their access credentials.

---

### 2. Background & Problem Statement

Currently, users at NFC Legal who need access to a specific application must raise a request through IT via email or verbal communication. There is no structured intake process, no visibility into request status, and credential delivery is manual. This creates IT bottlenecks, inconsistent provisioning, and a poor experience for end users. The goal is to replace this with a structured, automated, self-service portal.

---

### 3. Scope

**In Scope**
- A user-facing application catalog showing available applications
- A request submission flow for selecting an application and providing a business justification
- A configurable approval workflow routing the request to a designated approver
- Automated provisioning of application access upon approval
- Automated email delivery of credentials or access instructions to the requesting user
- Request status visibility for the end user

**Out of Scope**
- Managing SSO configuration or identity provider setup
- Provisioning access to applications not registered in the CF Manage application catalog
- Billing or cost allocation per application request

---

### 4. User Stories

- As an **end user at NFC Legal**, I want to browse available applications and request access through a portal so that I don't have to email IT and wait without visibility.
- As an **IT admin or approver**, I want to receive structured access requests with business justification so that I can approve or deny them quickly from a single interface.
- As a **CF Manage system**, I want to automatically provision access and deliver credentials upon approval so that no manual steps are required after the approver acts.
- As a **requesting user**, I want to receive an email with my credentials immediately after my request is approved so that I can get to work without chasing IT.

---

### 5. Functional Requirements

| ID | Requirement |
|----|-------------|
| F1 | The portal shall display a searchable catalog of all applications registered in CF Manage |
| F2 | Each application listing shall include: app name, description, owner, and estimated approval time |
| F3 | Users shall be able to submit an access request by selecting an application and entering a business justification |
| F4 | Upon submission, the request shall be routed to the configured approver for that application |
| F5 | The approver shall receive an email notification with request details and a one-click approve/deny action |
| F6 | The approver shall also be able to act on requests from within the CF Manage admin interface |
| F7 | Upon approval, CF Manage shall automatically provision access to the application for the requesting user |
| F8 | Upon provisioning, the system shall send the user an automated email containing their credentials or access instructions |
| F9 | The requesting user shall be able to view the live status of their request (Pending, Approved, Denied, Provisioned) |
| F10 | Upon denial, the user shall receive an email with the reason provided by the approver |
| F11 | All requests, approvals, denials, and provisioning events shall be logged in the CF Manage audit trail |
| F12 | Approvers shall be configurable per application, with support for single approver or sequential multi-approver chains |

---

### 6. Non-Functional Requirements

- **Performance:** The application catalog must load within 3 seconds for up to 50 registered applications
- **Security:** Credential delivery emails must be sent over encrypted channels; credentials must not be stored in CF Manage logs
- **Usability:** The portal must be accessible without requiring end users to have a CF Manage admin account
- **Auditability:** Every action in the request lifecycle must be logged with actor identity and timestamp

---

### 7. Workflow Diagram (Described)

1. User logs into portal → 2. Browses application catalog → 3. Selects app + submits justification → 4. Request routed to approver → 5. Approver receives email notification → 6a. Approved: CF Manage provisions access → credential email sent to user → status updated to Provisioned → 6b. Denied: denial email sent to user with reason → status updated to Denied → 7. All events logged in audit trail

---

### 8. Acceptance Criteria

- A user can submit an access request from the portal without IT involvement
- The approver receives an email notification within 2 minutes of request submission
- Approving a request results in access being provisioned and a credential email sent to the user within 5 minutes
- The user can view their request status in real time on the portal
- Denying a request sends the user a notification with the approver's reason
- All lifecycle events appear in the CF Manage audit log

---

### 9. Dependencies

- Application catalog must be populated with NFC Legal's registered applications prior to portal launch
- Approver assignments must be configured per application during onboarding
- Email delivery service must be active and configured within CF Manage
- Application APIs must support programmatic access provisioning for automation to work end-to-end

---

### 10. Open Questions

- Should the portal be accessible via browser only, or also via the CF Manage browser extension?
- Does NFC Legal require multi-level approval (e.g. manager + IT) for certain applications?
- How should credentials be handled for applications that use SSO vs. username/password?
- Should there be an automatic access expiry/review mechanism (e.g. access reviewed every 90 days)?

---

*CloudFuze | Internal + Client Facing | NFC Legal | March 2026*

## Comprehensive app development prompt with exceeds expectations rubric

```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

Phase 2 now includes the full Firebase iOS setup — project config, `app.json` settings with all the required iOS permissions, Firestore security rules, EAS Build steps for competition day, and the backup video plan. When you paste this into Claude Code, it'll handle both the Firebase integration and the onboarding flow in one go.

## Agentic SaaS management platform architecture

```
This block is not supported on your current device yet.
```

Now let me read both uploaded documents:
```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```

Now I have everything I need. Let me create the verification report as a professional docx:
```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```

The same `]]))` issue. Let me fix all occurrences at once:
```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```

Here's the full verification report. The summary of findings:

---

**Verdict: Conditionally Approved**

**What's solid:**
- The core technology stack (PostgreSQL + RLS, Nango, Fastify, Claude API) is well-chosen and production-proven
- The data model is well-designed with correct multi-tenant patterns and appropriate entity relationships
- The event schema standard (with `correlation_id`, `org_id`, `envelope`) is well-considered
- The API design is clean and follows conventions any engineer can work with
- ADR-001's analysis is accurate and should be treated as authoritative on scope

**Critical issues found:**
- **Over-scoped timeline** — the full architecture is 4–6 months of work, not 1 month
- **Missing Month-1 architecture** — the doc only shows the target state, which may mislead the team into building everything at once
- **No testing strategy** — a multi-tenant SaaS platform with no defined test plan is a data breach risk
- **No cost estimates** — Month-1 PaaS (~$200–500/mo) vs full K8s (~$2K–5K/mo) should be documented
- **pgvector included with no use case** — remove it from Month-1 schema until an embedding use case is defined
- **Email scanning (discovery source) needs legal review** — GDPR implications before enabling

**5 non-negotiable conditions to proceed:**
1. Adopt the Month-1 stack (BullMQ, Postgres events, PaaS, direct Claude API)
2. At least 1 senior engineer as Tech Lead — Claude Code cannot catch multi-tenancy bugs
3. Write cross-tenant RLS integration tests on Day 1
4. Mandatory code reviews on every PR
5. Lock MVP scope after kickoff — all new requests go to Month-2 backlog

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
- `trigger_workflow` — start an onboarding/offboarding/reclamation job via BullMQ. The agent should ALWAYS confirm with the user before calling this for destructive actions.
- `send_notification` — send a Slack message to a user or channel

Each tool is a JSON schema that Claude understands. On the backend, each maps to a function that runs a Prisma query or enqueues a BullMQ job.

### Streaming to the Chat UI

Use **Server-Sent Events (SSE)** from your Fastify API to the Next.js frontend. The **Vercel AI SDK** (`ai` npm package) is the fastest way to build this — it has:

- `useChat` hook for the frontend (handles streaming, message state, retry)
- Server-side streaming helpers that work with Claude API
- Built-in support for tool-use rendering (showing which tools are being called)

The flow: Next.js frontend → SSE connection to Fastify `/api/agent/chat` endpoint → Fastify runs the ReAct loop → streams text chunks and tool execution status back over SSE → frontend renders in real-time.

While the agent is "thinking" or executing a tool, you stream status updates: "Checking license data for Figma..." so the user sees progress.

### Conversation Management

- Store conversation history in PostgreSQL (`chat_sessions` and `chat_messages` tables, scoped by `org_id`)
- On each new message, load the last N messages from the session and include them in the Claude API call
- Claude has a 200K context window — you won't hit limits in month 1. If conversations get very long, summarize older messages (month 2 optimization)
- System prompt includes: org name, user's role, available apps count, and a brief description of what the agent can do

### Confirmation Flow for Destructive Actions

When a user says "offboard sarah@company.com":
1. Claude calls `query_users` to look up Sarah and her app assignments
2. Claude sees Sarah has 7 apps assigned
3. Instead of calling `trigger_workflow` immediately, Claude responds with text: "Sarah has access to 7 apps: Okta, Google Workspace, Slack, GitHub, Figma, Linear, Notion. Offboarding will revoke all access. Should I proceed?"
4. User replies "yes"
5. Claude calls `trigger_workflow` with `{type: "offboarding", userId: "sarah-id"}`
6. The workflow starts, and status updates stream back to chat

This is handled purely through Claude's system prompt — you instruct it to always confirm before destructive actions. No special tool needed.

---

## 2. Workflow Architecture (BullMQ)

### How Workflows Run

Each workflow is a **BullMQ job** that contains multiple steps. You have two patterns:

**Simple pattern (month 1):** One BullMQ job per workflow. The job function runs steps sequentially, saving progress to the database after each step. If it fails, BullMQ retries from the beginning, but each step checks if it already completed (idempotency

## SaaS agent platform MVP planning and deployment strategy

This is a really interesting design challenge. You're essentially building two interfaces for workflows:

1. **Chat-first creation** — the agent generates a workflow from natural language
2. **Visual canvas** — the user sees the workflow as a visual flow diagram and can drag, edit, rearrange

And they need to stay in sync. Let me think through this.

---

## The Core Design Concept

Think of it as **two views of the same data**. The workflow definition lives in your database as a structured JSON object. The chat agent creates and modifies that JSON. The canvas renders that JSON visually. The visual editor modifies that same JSON. Either side can make changes and the other side reflects them.

The mental model is similar to how Notion works — you can type in a document or drag blocks around, and it's all the same underlying data.

---

## The Workflow Data Structure

Before talking about UI, you need the right data format that both the agent and the canvas can work with. A workflow is essentially a **directed graph of steps**:

Each workflow has:
- A **trigger** (what starts it — HR event, schedule, manual via chat, webhook)
- A list of **steps** (each step is an action — provision an app, send a notification, wait for approval, check a condition)
- **Connections** between steps (step A flows to step B, or branches to B or C based on a condition)
- **Metadata** per step (which Nango integration to call, what parameters to pass, retry config)

This maps naturally to a JSON structure like:

```
{
  trigger: { type: "event", event: "hr.employee.created" },
  steps: [
    { id: "1", type: "lookup", action: "get-role-apps", position: {x, y} },
    { id: "2", type: "action", action: "provision-okta", position: {x, y} },
    { id: "3", type: "action", action: "provision-google", position: {x, y} },
    { id: "4", type: "condition", check: "all-succeeded", position: {x, y} },
    { id: "5", type: "notify", channel: "slack", position: {x, y} },
  ],
  edges: [
    { from: "1", to: "2" },
    { from: "1", to: "3" },  // parallel
    { from: "2", to: "4" },
    { from: "3", to: "4" },
    { from: "4", to: "5" },
  ]
}
```

The `position` field is what the canvas uses for layout. The `steps` and `edges` are what the execution engine uses. The agent reads and writes this entire structure.

---

## Frontend Approach: Three Panels

Picture the screen layout:

**Left panel: Chat** — Full conversation with the agent. This is where you describe what you want in natural language. The agent creates and modifies workflows here.

**Center panel: Canvas** — Visual flow diagram of the workflow. Nodes are steps, arrows are connections. You can drag nodes, add new ones, delete them, reconnect edges. This is the visual editor.

**Right panel: Step detail** — When you click a node on the canvas, this shows the configuration for that step (which app, what action, what parameters, retry settings, timeout).

All three panels operate on the same workflow JSON. Change something in chat → canvas updates. Drag something on canvas → the data updates. Click a node → details panel opens.

---

## How the Agent Creates a Workflow

User opens the workflow builder and says in the chat panel:

"Create an onboarding workflow for the engineering team. It should provision Okta, GitHub, Slack, and Linear. Send a welcome message to the #engineering channel when done. If any provisioning fails, notify the IT admin."

The agent:

1. **Parses the intent** — this is a new workflow, trigger type is onboarding, target is engineering role
2. **Calls a `generate_workflow` tool** — this tool returns a structured workflow JSON (the agent doesn't generate the JSON directly in its response, it uses a tool that validates the structure)
3. **The tool result contains the full workflow definition** with steps, edges, and reasonable defaults
4. **The frontend receives this** and renders it on the canvas immediately
5. **The agent narrates what it built**: "I've created an onboarding workflow with 6 steps. The flow starts with a role lookup, then provisions Okta, GitHub, Slack, and Linear in parallel. After all succeed, it sends a welcome message to #engineering. I added a failure branch that notifies your IT admin channel if any step fails. You can see it on the canvas — feel free to rearrange or edit any step."

The user now sees the full workflow visually on the canvas. They didn't touch a single form field or drag anything. The agent did all the work.

---

## How the User Edits on Canvas

Now the user looks at the canvas and thinks "I also want to add AWS Console access, and I want the Slack notification to go to #new-hires instead of #engineering."

They have two choices:

**Option A: Tell the agent.**
"Add an AWS Console provisioning step after Okta, and change the Slack channel to #new-hires."

The agent modifies the workflow JSON, the canvas updates.

**Option B: Edit directly on canvas.**
They drag a new node from a sidebar palette (a library of available step types — provision app, send notification, wait, condition, approval). They drop it between Okta and the completion check. They click it, select "AWS Console" from a dropdown in the details panel, and configure the role. They click the Slack notification node, change the channel field.

Both approaches modify the same underlying JSON. The canvas is just a visual editor for that JSON.

---

## Tech Stack for the Canvas

This is the key technical decision. You need a **node-based visual editor** in React. There are three realistic options:

### React Flow (recommended for Month 1)

React Flow is the most mature, most widely used node-based graph editor for React. It's what most workflow builders are built on today — n8n, Langflow, Flowise, and dozens of internal tools use it.

What it gives you: draggable nodes, connectable edges, zoom/pan, minimap, custom node components (so each step type can look different), auto-layout algorithms, and it's well-documented. Your junior frontend engineer can get productive with it in 1-2 days, especially with Claude Code helping.

It works with shadcn/ui and TailwindCSS (your existing stack). The nodes are React components, so you style them with Tailwind like anything else.

### XYFlow (React Flow v12+)

This is the newer version of React Flow, rebranded. Same team, same concept, more features. Check which version is current when you start — if v12+ is stable, use it. If not, stick with React Flow v11.

### Excalidraw-style (too custom for Month 1)

Some teams build canvas UIs from scratch using something like tldraw or Excalidraw's engine. This gives maximum flexibility but is a massive engineering effort. Not viable in 30 days.

**Go with React Flow.** It's the default choice for a reason.

---

## How the Canvas and Chat Stay in Sync

This is the trickiest part architecturally. You need **bidirectional sync** between the chat agent and the canvas editor.

**State ownership:** The workflow JSON lives in React state (probably a Zustand store or React context, since your whole app is Next.js). Both the chat and the canvas read from and write to this store.

**Chat → Canvas flow:**
1. Agent calls the `generate_workflow` or `modify_workflow` tool
2. The tool handler on the backend returns the updated workflow JSON
3. The frontend receives this via SSE (as part of the agent's tool result)
4. The frontend updates the Zustand store
5. React Flow re-renders with the new nodes and edges
6. Optional: animate the changes so the user sees what was added/modified

**Canvas → Chat flow:**
1. User drags a node, adds an edge, or modifies a step's config
2. The Zustand store updates
3. You DON'T need to send this back to the agent immediately — the agent only needs the current state when it next makes a decision
4. When the user next talks to the agent, the current workflow JSON is included in the agent's context (as part of the system prompt or a tool result)
5. So the agent always sees the latest version of the workflow, whether it was modified via chat or canvas

The key principle: **the agent doesn't maintain its own copy of the workflow.** Every time the agent needs to reason about the workflow, it reads the current state from the database or from the tool. This means canvas edits are automatically visible to the agent.

---

## Step Types (The Node Palette)

On the canvas, users can drag these types of nodes:

**Trigger nodes** (the starting point):
- HR Event (new hire, termination, role change)
- Schedule (daily, weekly, custom cron)
- Manual (triggered via chat or dashboard button)
- Webhook (external system calls your API)

**Action nodes** (do something):
- Provision App (dropdown: select from org's connected apps via Nango)
- Deprovision App
- Send Notification (Slack channel or DM)
- Update Database Record (change license status, user assignment)

**Logic nodes** (control flow):
- Condition (if/else — "if role is engineering, go left; otherwise go right")
- Parallel Split (run multiple branches simultaneously)
- Parallel Join (wait for all parallel branches to complete)
- Wait (delay for X hours/days — used for reclamation grace periods)
- Approval Gate (pause workflow, send approval request, resume on approval)

**Utility nodes:**
- AI Classify (send data to Claude for classification — useful for shadow IT assessment)
- HTTP Request (call any external API — month 2 feature, but design the node type now)

Each node type has its own custom React Flow node component with the right config fields in the detail panel.

---

## The Really Powerful Interaction

Here's where chat + canvas becomes more than the sum of its parts.

**Scenario 1: Agent explains a visual workflow**

User is looking at a complex workflow someone else built. They ask the agent: "Explain what this workflow does step by step."

The agent reads the workflow JSON, and walks through it in natural language: "This workflow triggers when someone leaves the company. First it suspends their SSO, then in parallel it deactivates their accounts across 6 apps. After all deactivations complete, it checks if they had any admin-level access anywhere. If they did, it sends an alert to the security team. Finally, it generates a compliance record and notifies their manager."

As the agent describes each step, the corresponding node on the canvas could **highlight** — creating a guided tour effect.

**Scenario 2: Agent debugs a failed workflow**

A workflow run failed. The user asks "why did the marketing onboarding fail for Lisa?"

The agent looks up the execution record, sees that step 4 (provision HubSpot) failed with an API error, and says: "Lisa's onboarding completed 4 of 5 steps. HubSpot provisioning failed because the HubSpot API returned a 'seat limit exceeded' error — you're at 50/50 seats. The other 4 apps (Okta, Google, Slack, Canva) were provisioned successfully. Want me to retry after you free up a HubSpot seat?"

On the canvas, the failed step shows in red with the error details.

**Scenario 3: Agent suggests workflow improvements**

After a workflow has run 50 times, the agent could analyze the execution data and say: "I've noticed that Google Workspace provisioning takes an average of 45 seconds and is always followed by Slack provisioning which takes 30 seconds. These are currently running sequentially. If I make them parallel, onboarding time would drop from ~3 minutes to ~1.5 minutes. Want me to update the workflow?"

The user says yes, the agent restructures the workflow JSON, and the canvas updates to show the newly parallelized steps.

---

## What to Build in Month 1 vs Later

**Month 1 (MVP):**
- Chat panel that can create and describe workflows
- Canvas that renders the workflow visually using React Flow (read-only or basic editing)
- 3-4 node types: Provision App, Deprovision App, Send Notification, Condition
- Agent can generate onboarding and offboarding workflows from natural language
- Agent can explain any workflow in natural language
- Real-time execution progress shown on canvas nodes (green/yellow/red status)

**Month 2:**
- Full drag-and-drop editing on canvas (add nodes from palette, reconnect edges)
- Detail panel for configuring each step
- Approval gate nodes (human-in-the-loop)
- Wait/delay nodes
- Canvas ↔ chat bidirectional editing
- Workflow version history

**Month 3+:**
- Agent suggests workflow optimizations based on execution data
- Workflow templates marketplace
- Custom HTTP request nodes
- Branching logic builder in the detail panel
- Canvas highlighting while agent explains steps

---

## The Honest Scope Question

With 1 senior and 4 juniors in 30 days, building a full bidirectional chat + canvas workflow builder is extremely ambitious. Here's what I'd actually recommend for Month 1:

**Chat-first, canvas read-only.** The agent creates workflows. The canvas shows them visually. But editing happens through the agent, not by dragging on the canvas. This cuts the frontend complexity in half — React Flow in read-only mode with custom styled nodes is much simpler than a full interactive editor with validation, undo/redo, and drag-from-palette.

Your frontend junior can build a beautiful read-only canvas with execution status overlays in about a week. A full interactive editor with all the UX polish would take 3-4 weeks from a mid-level engineer.

The chat does the heavy lifting in Month 1. The canvas becomes interactive in Month 2.

---

What do you want to explore next — the deployment/infrastructure setup, or should we go deeper on any part of this workflow + canvas design?

## Webhook trigger not firing from server

Here's how to set up **email notifications** in Jenkins:

---

**Step 1: Configure SMTP in Jenkins**

Go to **Manage Jenkins → System → Email Notification**
```
SMTP Server:    smtp.gmail.com          # or your company SMTP
SMTP Port:      465 (SSL) / 587 (TLS)
Use SSL:        ✅ checked
Username:       your-email@gmail.com
[REDACTED]       # Gmail needs App Password not real password
```
Click **Test Configuration** to verify.

---

**Step 2: Add Email in Jenkinsfile**

**Basic - Email on Failure only:**
```groovy
post {
    failure {
        mail to: 'team@cloudfuze.com',
             subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Build failed. Check: ${env.BUILD_URL}"
    }
}
```

**All status notifications:**
```groovy
post {
    success {
        mail to: 'team@cloudfuze.com',
             subject: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Build succeeded!\nJob: ${env.JOB_NAME}\nBuild: ${env.BUILD_URL}"
    }
    failure {
        mail to: 'team@cloudfuze.com',
             subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Build failed!\nJob: ${env.JOB_NAME}\nBuild: ${env.BUILD_URL}"
    }
    always {
        mail to: 'team@cloudfuze.com',
             subject: "Build ${currentBuild.result}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
             body: "Status: ${currentBuild.result}\nCheck: ${env.BUILD_URL}"
    }
}
```

---

**Step 3: Full Jenkinsfile Example**

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // your build steps
            }
        }
        stage('Deploy') {
            steps {
                // your deploy steps
            }
        }
    }

    post {
        success {
            mail to: 'team@cloudfuze.com',
                 subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """
                     Build deployed successfully!
                     
                     Job     : ${env.JOB_NAME}
                     Build # : ${env.BUILD_NUMBER}
                     Branch  : ${env.GIT_BRANCH}
                     URL     : ${env.BUILD_URL}
                 """
        }
        failure {
            mail to: 'team@cloudfuze.com',
                 subject: "❌ FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """
                     Build/Deploy FAILED!
                     
                     Job     : ${env.JOB_NAME}
                     Build # : ${env.BUILD_NUMBER}
                     Branch  : ${env.GIT_BRANCH}
                     URL     : ${env.BUILD_URL}
                     
                     Please check the logs immediately.
                 """
        }
    }
}
```

---

**For Gmail specifically — App Password setup:**
1. Go to Google Account → **Security**
2. Enable **2-Step Verification**
3. Go to **App Passwords** → Generate one for Jenkins
4. Use that 16-digit password in Jenkins SMTP config

---

Do you want notifications on **Slack or Teams** as well? That's also very easy to set up alongside email.

## Webhook trigger not firing from server

Let me analyze what's needed:

1. There's a Bitbucket repo `content.git` with a branch (develop/master)
2. When code is pushed, Jenkins job `Content_Develop_Content4x` should build automatically
3. After build, the WAR file should be automatically deployed to all Tomcat instances under `/mnt/content4x`
4. Currently done manually using `shutdown2.sh` and `startup1.sh`
5. Need to automate this completely

The existing flow:
- Developer commits code
- Jenkins job `Content_Develop_Content4x` builds WAR
- Manually download WAR using `shutdown2.sh` script


- Deploy to all tomcat instances
- Start using `startup1.sh`

So the new automated flow should be: developer commits to Bitbucket, which triggers Jenkins via webhook, builds the WAR, then automatically deploys it across all Tomcat instances. I need to set up the Bitbucket webhook, configure Jenkins to listen for it, and create a post-build deployment script that pulls the WAR directly from the Jenkins workspace, stops all Tomcat instances, deploys to each webapps directory, and restarts them.

The key difference from the crawl setup is that there's no Docker involved here—just multiple Tomcat instances running under `/mnt/content4x`, and I can grab the WAR directly from the Jenkins workspace since it's on the same server or accessible via SSH, so no wget needed. The Jenkins job `Content_Develop_Content4x` builds the WAR to its workspace directory, and I need to write a script that locates that built artifact and deploys it to all the Tomcat instances. I need to verify the network connectivity between these servers—Jenkins on `cfjenkins.cloudfuze.com` and the deployment server `DXDS3138` at 208.70.248.71—but since Jenkins already handles SSH connections to the deployment server for the crawl jobs, I can use the same approach here. Now I'm writing out the exact deployment script and Jenkins configuration steps.Let me analyze your setup clearly first:

**Current manual flow:**
```
Code commit → Jenkins builds WAR → Manually run shutdown2.sh → wget WAR → startup1.sh
```

**New automated flow:**
```
Code commit → Bitbucket webhook → Jenkins builds WAR → Auto deploy to all Tomcat instances
```

---

## PHASE 1: Create Auto Deploy Script on Deployment Server

```bash
cat > /home/pavan/crawl/git/script-content-develop.sh << 'EOF'
#!/bin/bash
set -e

echo "============================================"
echo " Content4x - Auto Deploy Script"
echo " Started at: $(date)"
echo "============================================"

# ── VARIABLES ──────────────────────────────────
BASE_DIR="/mnt/content4x"
JENKINS_WAR_URL="https://cfjenkins.cloudfuze.com/job/Content_Develop_Content4x/ws/modules/CloudFuzeWeb/target/cloudfuze.war"
JENKINS_USER="hymavathi"
JENKINS_PASS="your-jenkins-[REDACTED]="/home/pavan/crawl/git"
WAR_FILE="$WAR_DOWNLOAD_DIR/cloudfuze.war"

# ── STEP 1: STOP ALL TOMCAT INSTANCES ──────────
echo ""
echo "=== STEP 1: Stopping all Tomcat instances ==="
for full_dir in "$BASE_DIR"/*/; do
    if [ -f "${full_dir}bin/shutdown.sh" ]; then
        echo "Stopping Tomcat in $full_dir..."
        sh "${full_dir}bin/shutdown.sh" -force 2>/dev/null || true
        echo "✅ Stopped: $full_dir"
    fi
done
sleep 10
echo "✅ All Tomcat instances stopped"

# ── STEP 2: CLEAN OLD WEBAPPS ──────────────────
echo ""
echo "=== STEP 2: Cleaning old webapps ==="
for full_dir in "$BASE_DIR"/*/; do
    if [ -d "${full_dir}webapps/cloudfuze" ]; then
        rm -rf "${full_dir}webapps/cloudfuze"
        echo "✅ Cleaned: ${full_dir}webapps/cloudfuze"
    fi
done

# ── STEP 3: DOWNLOAD NEW WAR ───────────────────
echo ""
echo "=== STEP 3: Downloading new WAR from Jenkins ==="
rm -f "$WAR_FILE"
wget --auth-no-challenge \
     --http-user="$JENKINS_USER" \
     --http-password="$JENKINS_PASS" \
     -O "$WAR_FILE" \
     "$JENKINS_WAR_URL"

if [ $? -ne 0 ]; then
    echo "❌ WAR download failed! Aborting."
    exit 1
fi
echo "✅ WAR downloaded successfully"
ls -lh "$WAR_FILE"

# ── STEP 4: BACKUP & DEPLOY WAR ────────────────
echo ""
echo "=== STEP 4: Deploying WAR to all Tomcat instances ==="
for full_dir in "$BASE_DIR"/*/; do
    if [ -d "${full_dir}webapps" ]; then
        # Backup existing WAR
        if [ -f "${full_dir}webapps/cloudfuze.war" ]; then
            mv "${full_dir}webapps/cloudfuze.war" "${full_dir}webapps/cloudfuze.war-bak"
            echo "📦 Backed up: ${full_dir}webapps/cloudfuze.war"
        fi
        # Copy new WAR
        cp "$WAR_FILE" "${full_dir}webapps/cloudfuze.war"
        echo "✅ Deployed to: ${full_dir}webapps/"
    fi
done

# ── STEP 5: START ALL TOMCAT INSTANCES ─────────
echo ""
echo "=== STEP 5: Starting all Tomcat instances ==="
for dir in "$BASE_DIR"/*/; do
    if [ -f "${dir}bin/startup.sh" ]; then
        sh "${dir}bin/startup.sh"
        echo "✅ Started: $dir"
    fi
done
sleep 15

# ── STEP 6: VERIFY ────────────────────────────
echo ""
echo "=== STEP 6: Verifying deployments ==="
echo "Checking WAR versions deployed:"
cat /mnt/content4x/*/webapps/cloudfuze/META-INF/MANIFEST.MF 2>/dev/null | grep Implementation-Version || echo "Waiting for WAR to extract..."

echo ""
echo "Checking running Tomcat processes:"
ps aux | grep tomcat | grep -v grep | wc -l
echo "Tomcat processes running"

echo ""
echo "============================================"
echo " ✅ DEPLOYMENT COMPLETE!"
echo " Finished at: $(date)"
echo "============================================"
EOF
```

Make it executable:
```bash
chmod +x /home/pavan/crawl/git/script-content-develop.sh
```

---

## PHASE 2: Set Up Bitbucket Webhook

**Go to Bitbucket → `content` repo → Settings → Webhooks → Add webhook:**

```
Title:   Jenkins-Content-Develop
URL:     https://cfjenkins.cloudfuze.com/generic-webhook-trigger/invoke?[REDACTED]
Triggers: Repository push ✅
Active:  ✅
```

---

## PHASE 3: Configure Jenkins Job

Go to `https://cfjenkins.cloudfuze.com/job/Content_Develop_Content4x/configure`

**3a. Build Triggers section — check Generic Webhook Trigger:**
```
[REDACTED]
```

**3b. Source Code Management:**
```
Repository URL: git@bitbucket.org:cfdeveloper/content.git
Branch: */develop    ← (or master, confirm which branch)
```

**3c. Build Steps — confirm Maven build is configured:**
```
Goals: clean package -DskipTests
POM: modules/CloudFuzeWeb/pom.xml
```

**3d. Post-build Actions → Add "Send files or execute commands over SSH":**
```
SSH Server:  DXDS3138 (208.70.248.71)
Exec command: bash /home/pavan/crawl/git/script-content-develop.sh
```

---

## PHASE 4: Test Deploy Script Manually First

**SSH into deployment server and test:**
```bash
bash /home/pavan/crawl/git/script-content-develop.sh
```

Verify all versions updated:
```bash
cat /mnt/content4x/*/webapps/cloudfuze/META-INF/MANIFEST.MF | grep Implementation-Version
```

All should show the **new build number** (125 or latest).

---

## PHASE 5: Test Full CI/CD Pipeline

**Trigger with a code push:**
```bash
cd /home/pavan/crawl/git/content-develop
echo "ci-cd-test-$(date)" >> README.md
git add .
git commit -m "test: auto deploy trigger"
git push origin develop
```

**Watch Jenkins job:**
```
https://cfjenkins.cloudfuze.com/job/Content_Develop_Content4x/
```

---

## PHASE 6: Verify After Deployment

```bash
# Check all WAR versions match new build
cat /mnt/content4x/*/webapps/cloudfuze/META-INF/MANIFEST.MF | grep Implementation-Version

# Check all Tomcat processes running
ps aux | grep tomcat | grep -v grep

# Check specific job responding
curl -I http://208.70.248.71:<port>/cloudfuze/
```

---

## ⚠️ Important — Fix Jenkins Password in Script

Replace `your-jenkins-password` with the actual password of `hymavathi` Jenkins user:

```bash
sed -i 's/your-jenkins-password/ACTUAL_PASSWORD/' /home/pavan/crawl/git/script-content-develop.sh
```

**Or better — use Jenkins API token instead of password:**
1. Login as `hymavathi` in Jenkins
2. Go to → User → Configure → API Token → Generate
3. Use that token as `JENKINS_PASS` in the script

---

**What is the branch name in the `content` repo you want to trigger on — `develop` or `master`?** That's the only thing to confirm before we finalize the webhook and job config.

## Webhook trigger not firing from server

The user has successfully configured the SSH connection. Now they need to complete the Jenkins job configuration for Content_Develop_Content4x.Now fill in the job configuration:

---

**Go to `Content_Develop_Content4x` → Configure → Post-build Actions → Send files or execute commands over SSH:**

```
Name:              71server          ← select from dropdown
Source files:                        ← leave empty
Remove prefix:                       ← leave empty
Remote directory:                    ← leave empty
Exec command:      bash /home/pavan/crawl/git/script-content-develop.sh
```

Click **Save**

---

**Then set up Build Trigger — go to Build Triggers section:**

```
☑ Generic Webhook Trigger
[REDACTED]
```

Click **Save**

---

**Then add webhook in Bitbucket:**

Go to `content` repo → **Settings → Webhooks → Add webhook:**
```
Title:    Jenkins-Content-Develop
URL:      https://cfjenkins.cloudfuze.com/generic-webhook-trigger/invoke?[REDACTED]
Triggers: ☑ Repository push
Status:   ☑ Active
```

---

**Now test manually first before pushing code:**

Go to `Content_Develop_Content4x` → **Build Now** → watch Console Output.

It should:
1. Build the WAR ✅
2. SSH into 208.70.248.71 ✅
3. Run `script-content-develop.sh` ✅
4. Deploy to all Tomcat instances ✅

Share the console output after clicking Build Now!

## How well do you know cloudfuze...

```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

Here's your markdown PRD. To get it into Google Docs or Word for editing:

**Google Docs:**
1. Open a new Google Doc
2. Copy the entire content from this artifact
3. Paste it in — Google Docs will preserve most of the formatting
4. Use **Format → Paragraph styles** to clean up headings if needed

**Microsoft Word:**
1. Open a new Word document
2. Copy and paste the content
3. Or save it as a `.md` file and open with Word — it will auto-convert

The markdown format also means you can paste it into **Notion, Confluence, or Linear** and it'll render perfectly too. Would you like any content changes before you take it across?
