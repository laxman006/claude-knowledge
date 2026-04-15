# Git Workflow

> Derived from Claude conversation history via ccmigrate.

## SaaS agent platform MVP planning and deployment strategy

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

With BullMQ, each workflow is a **job processor** — a TypeScript function registered with a queue. When the agent triggers a workflow, it adds a job to the queue with a payload. The worker picks it up and runs the steps.

Here's the onboarding workflow as an example:

```typescript
// workers/onboarding.worker.ts

import { Worker, Job } from "bullmq";
import { nangoClient } from "../lib/nango";
import { db } from "../lib/prisma";
import { publishProgress } from "../lib/progress";

interface OnboardingPayload {
  orgId: string;
  userId: string;
  email: string;
  role: string;
  department: string;
  chatSessionId: string; // so we can push updates back to chat
}

const onboardingWorker = new Worker("onboarding", async (job: Job<OnboardingPayload>) => {
  const { orgId, userId, email, role, department, chatSessionId } = job.data;

  // Step 1: Look up which apps this role needs
  await job.updateProgress({ step: "resolve-apps", status: "running" });
  const roleApps = await db.roleAppMapping.findMany({
    where: { orgId, role, department }
  });
  await publishProgress(chatSessionId, {
    step: "resolve-apps",
    status: "done",
    message: `Found ${roleApps.length} apps for ${role} role: ${roleApps.map(a => a.appName).join(", ")}`
  });

  // Step 2: Provision each app (sequential f

## Webhook trigger not firing from server

Here's the complete step-by-step guide:

---

**Step 1: Test Email Config First (Before touching the job)**

Go to **Manage Jenkins → System** → scroll down to **Extended E-mail Notification** → click **"Test configuration by sending test e-mail"**

Enter any email and click **Test** — if you get the email, SMTP is working ✅

---

**Step 2: Add Email Notification to Your Jenkinsfile**

Open your job's **Jenkinsfile** and add the `post` block:

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // your existing build steps
            }
        }
        stage('Deploy') {
            steps {
                // your existing deploy steps
            }
        }
    }

    // ADD THIS BLOCK AT THE END
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
                     
                     Please check logs immediately.
                 """
        }
        always {
            echo "Build finished with status: ${currentBuild.result}"
        }
    }
}
```

---

**Step 3: Enable for the Job**

1. Go to your **Jenkins Job → Configure**
2. Scroll down to **Post-build Actions**
3

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

## Webhook trigger not firing from server

The user ran git branch -a but didn't share the output. They're saying they're confused and it should be only for develop branch.You didn't paste the output! But no worries — since you confirmed it should be **develop branch**, do this:

---

**Step 1: Check and switch to develop branch:**
```bash
cd /home/pavan/crawl/git/content-develop
git checkout develop
git pull origin develop
```

---

**Step 2: Update Jenkins job branch:**

Go to `Content_Develop_Content4x` → **Configure → Source Code Management:**
```
Branch Specifier: */develop
```
Click **Save**

---

**Step 3: Update Bitbucket webhook filter (optional but recommended):**

Go to Bitbucket → `content` repo → **Settings → Webhooks → edit your webhook:**
```
URL: https://cfjenkins.cloudfuze.com/generic-webhook-trigger/invoke?[REDACTED]
```
This already triggers on any push — but Jenkins will only build `develop` branch now.

---

**Step 4: Test push:**
```bash
cd /home/pavan/crawl/git/content-develop \
&& git checkout develop \
&& echo "infratest9$(date)" >> README.md \
&& git add . \
&& git commit -m "test1: trigger check" \
&& git push origin develop
```

---

**Then watch Jenkins job trigger automatically at:**
```
https://cfjenkins.cloudfuze.com/job/Content_Develop_Content4x/
```

Run Step 1 first and share the output!

## Bitbucket code commits

Perfect choice! Here are the exact steps:
```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

Here's the complete guide using SCP shell commands. The key difference from before — **no extra plugins needed at all**.

**Do these in exact order:**

**STEP 1 — SSH Key Setup (most important)**
- Login to Jenkins server → switch to `jenkins` user → run `ssh-keygen` → run `ssh-copy-id ubuntu@208.70.252.58` → test that `ssh ubuntu@208.70.252.58` works **without any password**. If this doesn't work, nothing else will work.

**STEP 2 — Add SSH Credential in Jenkins UI**
- Manage Jenkins → Credentials → Global → Add Credentials → paste the private key from `/var/lib/jenkins/.ssh/id_rsa`

**STEP 3 — Configure Jenkins Job**
- SCM → Git → your Bitbucket repo → branch `*/release/1.0`
- Build trigger → BitBucket push
- Add **Execute Shell** build step → paste the exact shell script (it handles stop → remove old WAR → SCP new WAR → start)

**STEP 4 — Bitbucket Webhook**
- Add webhook URL `https://cfjenkins.cloudfuze.com/bitbucket-hook/` in Bitbucket repo settings

**Start with Step 1 right now** — run the SSH key commands and tell me if `ssh ubuntu@208.70.252.58` logs in without a password. That's the foundation everything depends on.
