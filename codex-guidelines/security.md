# Security

> Derived from Claude conversation history via ccmigrate.

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
- As a **CF Manage workflow engi

## Comprehensive app development prompt with exceeds expectations rubric

No, you don't need to manually download any SDK. When you run `npm install` with the Firebase packages in your `package.json`, everything gets pulled in automatically.

Claude Code will handle adding the right packages when you give it the Phase 1 prompt — it'll run something like:

```
npx expo install @react-native-firebase/app @react-native-firebase/auth @react-native-firebase/firestore @react-native-firebase/storage
```

The only manual steps you need to do yourself in the Firebase console are:

1. Create a Firebase project at firebase.google.com
2. Add an iOS app with your bundle ID
3. Download the `GoogleService-Info.plist` file and drop it in your project root
4. Turn on the services you need (Authentication, Firestore, Storage, Cloud Messaging) in the console dashboard

Everything else — the SDK, dependencies, linking — Expo and npm handle for you. Just make sure you have the Firebase project created and the plist file in place before you start Phase 2, since the auth setup will need it to actually connect.

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
          

## Webhook trigger not firing from server

The SSH auth is failing. They need to set up SSH key-based auth between Jenkins and the deployment server.This is an SSH authentication failure. Let's fix it step by step:

---

**Step 1: Check how your existing 4 jobs connect via SSH**

Go to **Manage Jenkins → System → Publish over SSH** — check how the existing working SSH server (used by CF_ContentsPrawl jobs) is configured. Use the exact same method.

---

**Step 2: Get Jenkins server's public key**

Run on **Jenkins server (cfjenkins.cloudfuze.com):**
```bash
cat ~/.ssh/id_rsa.pub
```

If no key exists, generate one:
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
cat ~/.ssh/id_rsa.pub
```

---

**Step 3: Add Jenkins public key to deployment server**

Run on **deployment server (208.70.248.71):**
```bash
mkdir -p ~/.ssh
echo "PASTE_PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

---

**Step 4: Test SSH manually from Jenkins server**

Run on **Jenkins server:**
```bash
ssh root@208.70.248.71 "echo connected"
```
Should print `connected` without asking password.

---

**Step 5: Update Jenkins SSH config**

Go to **Manage Jenkins → System → Publish over SSH → SSH Server:**

```
Name:       71server
Hostname:   208.70.248.71
Username:   root
```

Click **Advanced:**
```
☑ Use password authentication, or use a different key
Passphrase/Password:  (leave empty)
Path to key:          (leave empty)
Key: (paste content of ~/.ssh/id_rsa from Jenkins server)
```

Click **Test Configuration** → should say **Success** ✅

---

Run Step 2 first and share the output!

## Webhook trigger not firing from server

The issue is clear - the script ran in background with nohup but it's stopping all Tomcat instances and then failing somewhere (likely the wget to download WAR fails because it runs in background without proper auth, or the script exits early). The version is still 124, meaning the WAR download and deployment didn't complete.

Let me check the log file and fix the script.The script ran but **only stopped Tomcat and didn't complete** — because it's running in background. Let's check the log first:

---

**Step 1: Check what happened in the log:**
```bash
cat /home/pavan/crawl/git/deploy-content-develop.log
```

Share that output — it will show exactly where it failed.

---

**Step 2: The real issue is the wget needs Jenkins password**

Check if WAR exists on Jenkins workspace directly:
```bash
curl -u hymavathi:JENKINS_PASSWORD -I "https://cfjenkins.cloudfuze.com/job/Content_Develop_Content4x/ws/modules/CloudFuzeWeb/target/cloudfuze.war"
```

---

**Step 3: Instead of wget from script, copy WAR directly via Jenkins SSH**

This is more reliable. Update the Jenkins job Post-build Actions:

```
Source files:    modules/CloudFuzeWeb/target/cloudfuze.war
Remove prefix:   modules/CloudFuzeWeb/target
Remote directory: /home/pavan/crawl/git/
Exec command:    bash /home/pavan/crawl/git/script-content-develop.sh
```

---

**Then update `script-content-develop.sh` — remove the wget step entirely:**

```bash
nano /home/pavan/crawl/git/script-content-develop.sh
```

Change `STEP 3` to use the already-copied WAR:

```bash
# ── STEP 3: USE WAR COPIED BY JENKINS ──────────
echo ""
echo "=== STEP 3: Verifying WAR file ==="
WAR_FILE="/home/pavan/crawl/git/cloudfuze.war"

if [ ! -f "$WAR_FILE" ]; then
    echo "❌ WAR file not found at $WAR_FILE! Aborting."
    exit 1
fi
echo "✅ WAR file found: $(ls -lh $WAR_FILE)"
```

---

**Also remove nohup — run directly:**

In Jenkins Exec command change back to:
```bash
bash /home/pavan/crawl/git/script-content-develop.sh
```

And increase timeou
