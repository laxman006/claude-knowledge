# User Context: Tharun.Pothi@cloudfuze.com

> Derived from Claude conversation history via ccmigrate.
> Source: 2 conversations

## Work Context from Conversations

### Running Claude from the command line

# Yes! Let's Build a Chatbot That Queries Metabase Directly

You're absolutely right. We can build this **without a Java backend server** by querying Metabase API directly from the chatbot.

Let me show you the architecture:

---

## **Architecture: Chatbot → Metabase (No Java Server)**

```
┌─────────────────────────────────────────────────────────────┐
│ User Types Question in Chat                                 │
│ "How much data has migrated?"                               │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│ ChatBot Frontend (React / Next.js)                           │
│ - Chat interface                                            │
│ - LangChain.js (JavaScript A