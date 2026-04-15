# User Context: ravic@cloudfuze.com

> Derived from Claude conversation history via ccmigrate.
> Source: 2 conversations

## Work Context from Conversations

### Building a code review agent

```
This block is not supported on your current device yet.
```

Great choices. Here's the plan: we'll build a **VS Code extension concept** as an interactive Artifact that simulates the experience — with a code editor panel, a "Review" button that calls Claude's API, and a structured output panel showing findings grouped by category (bugs, security, style, performance).

This gives you the full architecture to understand how a real VS Code extension would work, but running right in your browser.
```
Viewing artifacts created via the Analysis Tool web feature preview isn’t yet supported on mobile.
```

The artifact is a fully functional VS Code-style code review agent. Here's what's inside:

**How it works:**
The artifact calls Claude's API directly with a system prompt that instructs it t

### Cron vs fixed delay scheduling

Java 21 has **Virtual Threads** — this is a game changer for schedulers!

---

### Enable Virtual Threads in Spring Boot (Java 21)

Simply add this to `application.properties`:
```properties
spring.threads.virtual.enabled=true
```

That's it! Spring Boot 3.2+ **automatically** uses virtual threads for scheduling.

---

### What are Virtual Threads?

| | Platform Threads (old) | Virtual Threads (Java 21) |
|---|---|---|
| Created by | OS | JVM |
| Weight | Heavy (~1MB each) | Extremely lightweight |
| Max threads | ~thousands | **Millions** |
| Blocking | Wastes OS thread | JVM parks it, no waste |
| Config needed | Yes (pool size) | ❌ No pool needed! |

---

### What this means for your schedulers

```
:00 → ContentSprawlScheduler  → runs on Virtual Thread 1 ✅
:00 → CollabsSprawlScheduler 