# Afterglow Mini

A lightweight conversational memory architecture designed around a **single API call for the ordinary response path**.

Afterglow Mini keeps the main chat loop small: persist the message, decide whether the bot should respond, assemble a bounded context, and call the main model once. Memory maintenance is deferred until the active context reaches a compact threshold.

![Afterglow Mini architecture](docs/afterglow%20mini%20pic.png)

## Context recipe

```text
System
+ Pinned Memories (<= 50)
+ Rolling Summary
+ Recent Raw (~12k)
+ Reply Target (optional)
+ Current Message
```

Non-triggering Telegram messages are stored in D1 with **no model call**.

When compaction is needed, a separate hidden call produces a new rolling summary plus validated `memory_ops`. It does not reply to the user.

## Architecture

See [Architecture](docs/architecture.md) for the full flow and storage lifecycle.

## Core idea

**Keep the reply path simple. Let memory maintenance happen only when it is actually needed.**
