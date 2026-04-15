# Architecture

> Derived from Claude conversation history via ccmigrate.

## Embedding AI agent in-app with streamlined architecture

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

Here's the architecture doc — landscape format for readability. It contains:

**6-layer architecture diagram** built as a visual layered table:
1. User Interface — Next.js dashboard + Chat Panel + Widget Renderer
2. API Gateway — Fastify/tRPC + the 3 new agent endpoints + Clerk JWT
3. Agent Core — Agent loop + Claude API + Tool Registry + Response Builder
4. Data Layer — PostgreSQL mirror tables vs MongoDB side by side, with the exact schema for each
5. Sync Bridge — Option A (Change Streams, recommended) and Option B (REST polling, zero Java changes) shown side by side with code snippets
6. Write-Back — Java REST API as the only write path, with audit trail

**Plus three reference sections:**
- Key design decisions table (6 decisions with rationale)
- What changes vs what stays the same (two-column comparison)
- 3-week implementation order with "done when" acceptance criteria per week

## History overview

Great question. Let me walk through it with a concrete example.

Say `channelId = "222"` and after Step 1 your `originalText` is:

`"Hello <#222> and <#999>"`

This line:
```java
originalText = originalText.replace("<#" + channelId + ">", channelPlaceholder);
```

builds the string `"<#222>"` and replaces it with `"@@CHANNEL_ID_PLACEHOLDER@@"`, giving:

`"Hello @@CHANNEL_ID_PLACEHOLDER@@ and <#999>"`

**Why do we need this?** Because the next line:
```java
originalText = originalText.replaceAll("<#(.*?)>", "$1");
```

strips **all** `<#...>` tags. Without the placeholder, it would turn `<#222>` into just `222`, and we'd lose the wrapping we want to keep.

With the placeholder in place, the regex only hits `<#999>` (stripping it to `999`), and then we restore the placeholder back to `<#222>` at the end.

So the placeholder acts as a **shield** — it hides the channelId reference from the regex so it doesn't get stripped like the others.

## History overview

Good catch — let's trace through when there's **no `<#rootId>` in the message**.

Say `rootId = "111"`, `channelId = "222"`, and `originalText = "Hello <#999> world"`.

**Step 1:** The `contains` check fails because there's no `<#111>` in the text. So nothing happens here — we skip the replace.

**Step 2:** This line runs:
```java
originalText = originalText.replace("<#" + channelId + ">", channelPlaceholder);
```
It looks for `"<#222>"` in the text, but it's **not there**. So `replace` does nothing — it just returns the original string unchanged.

**Step 3:** The regex runs and strips `<#999>` → `999`.

**Step 4:** The restore line looks for the placeholder, but it was never inserted, so again nothing happens.

**End result:** `"Hello 999 world"` — just the normal regex strip, exactly as before.

So yes, this code runs for **every message**, but when there's no channel mention to protect, the placeholder lines are essentially no-ops. They only have an effect when Step 1 actually inserted a `<#channelId>` that needs shielding.
