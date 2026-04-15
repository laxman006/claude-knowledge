# User Context: harshith.kaduluri@cloudfuze.com

> Derived from Claude conversation history via ccmigrate.
> Source: 3 conversations

## Work Context from Conversations

### Automating routine tasks

Great choices! Power Automate is perfect for this — no coding needed and it runs on Microsoft's cloud so it'll trigger automatically every day.

Here's the step-by-step guide to set this up:

---

## Power Automate: Metabase → Excel Online (Daily Sync)

### Overview of the Flow
```
Recurrence (Daily) → HTTP (Metabase Login) → HTTP (Fetch Question) → Excel (Add Row)
```

---

### Step 1: Create a new Flow
1. Go to [make.powerautomate.com](https://make.powerautomate.com)
2. Click **Create** → **Scheduled cloud flow**
3. Set it to run **once a day** at your preferred time
4. Name it something like `Metabase to Excel Daily Sync`

---

### Step 2: Authenticate with Metabase
Add an **HTTP** action:
- **Method:** `POST`
- **URI:** `http://mb.syncfuze.com/api/session`
- **Headers:** `Content-Type:

### How to get that

## Getting a Postman API Key

Here's how to get your Postman API key:

1. **Log in** to your Postman account at [postman.com](https://postman.com)

2. **Go to your Profile settings:**
   - Click your **avatar/profile icon** in the top-right corner
   - Select **"Settings"**

3. **Navigate to API Keys:**
   - Click on the **"API keys"** tab on the left sidebar

4. **Generate a new key:**
   - Click **"Generate API Key"**
   - Give it a **name** (e.g., "Claude MCP")
   - Click **"Generate"**

5. **Copy the key** — it will only be shown **once**, so make sure to save it somewhere safe before closing.

6. **Paste it** into the Postman API Key field in the Claude MCP configuration dialog and hit **Save**.

> ⚠️ **Important:** Never share your API key publicly, as it grants access to your Postma

### Admin access token refresh for cloud migration

```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```


```
This block is not supported on your current device yet.
```

Based on the search results, here's a comprehensive breakdown of which clouds/platforms require you to **refresh the admin access token** during cloud content migration:

---

## Clouds That Require Admin Access Token Refresh

### 1. **Atlassian Cloud (Jira / Confluence)**
When running the Jira Cloud Migration Assistant (JCMA) or the Confluence Cloud Migration Assistant (CCMA) to migrate from Server to Cloud, authentication tokens are set to expire in **21 days**. After 16 days, the Migration Assistant will show a warning message about the token expiring in 5 days, and on the 21st day, the prefli