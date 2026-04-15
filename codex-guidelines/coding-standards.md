# Coding Standards

> Derived from Claude conversation history via ccmigrate.

## Understanding and implementing gstack for your team

The user is on platform.claude.com login page and sees "Continue with Google" and "Continue with email" options. There's no separate "Sign Up" button - the "Continue with email" button handles both login and signup. I gave wrong info saying "click Sign Up." Let me correct this and give accurate guidance.

They should just click "Continue with email" with their pavan@cloudfuze.com email already filled in - that will either log them in or create an account.You're right, I'm sorry for the wrong info! There is NO separate signup button. It's combined. Here's what to do RIGHT NOW on that exact screen:

---

## Do This Right Now — 3 Simple Steps

### Step 1: That email is already filled in ✅
I can see **pavan@cloudfuze.com** is already in the email box.

### Step 2: Click **"Continue with email"** button
That big black button. Click it.

### Step 3: Check your inbox IMMEDIATELY
A verification code will arrive in **30-60 seconds** to pavan@cloudfuze.com.

---

## If Code Still Doesn't Arrive — Do This Instead

Since your email is a Microsoft/Outlook account, try this faster option:

**Click "Continue with Google"** — but wait, your email is Outlook not Gmail.

So instead, ask your IT admin to **whitelist this sender** in your Microsoft 365 mail gateway:
```
no-reply-4HQgDYVUYkSCyRENbJLFkw@mail.anthropic.com
```

I can actually see this exact sender address in your earlier email screenshot! That's the exact address Anthropic sends from. Your IT admin whitelists that → code arrives instantly.

---

## Fastest Option Right Now

Go to your **Microsoft 365 quarantine** — your company might be holding the email there:

1. Go to **https://security.microsoft.com**
2. Click **"Review" → "Quarantine"**
3. Search for emails from **anthropic.com**
4. Release it to your inbox

That quarantine is most likely where your verification code is sitting right now! 🎯
