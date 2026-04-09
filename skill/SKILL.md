---
name: session-continuity
description: >
  Automatically resume context from previous conversations at the start of every new chat session.
  ALWAYS trigger this skill when: the user opens a new conversation and says something like "hi",
  "hey", "good morning", "let's continue", "where were we", "what were we working on",
  "pick up from last time", "continue from yesterday", "what's my current status",
  "resume", "catch me up", or any casual greeting that implies a returning user.
  Also trigger when the user references "last time", "previously", "before", or "our last chat"
  without specifying the topic. The goal is to make every new session feel like a continuation,
  not a fresh start. Do NOT trigger for clearly first-time or topic-specific questions with full context given.
---

# Session Continuity Skill

This skill makes Claude behave like a persistent collaborator — one who remembers what you were
working on, where you left off, and what's coming up next. Every new session starts with a
structured context reload, not a blank slate.

---

## When to Trigger

Trigger on any session-opening message that implies a returning user:

- Simple greetings: "hi", "hey", "good morning", "hello Claude"
- Continuation signals: "let's continue", "pick up where we left off", "where were we"
- Status checks: "what's my current status", "what am I working on", "catch me up"
- Vague references to past work: "that thing we were doing", "the project", "the email we drafted"
- Any opener where the user clearly expects you to already know their context

---

## Step-by-Step Execution

### Step 1 — Load Recent Chats

Call `recent_chats` with `n=5` to retrieve the most recent conversations.

```
recent_chats(n=5, sort_order="desc")
```

From these results, extract:
- **Active threads**: What topics/projects were discussed in the last 1–3 days?
- **Pending items**: Were there any tasks left incomplete, questions unanswered, or "I'll do this later" moments?
- **Decisions made**: Key choices or directions confirmed
- **Waiting-on items**: Things the user said they'd follow up on (e.g. "I'll check and come back")

### Step 2 — Identify Hot Topics

Scan the chat summaries for recurring themes. If a topic appears in 2+ of the last 5 chats,
it's a "hot topic" — something actively in motion. Examples:

- An ongoing internship application campaign
- A project being built (e.g. an automation workflow)
- A recurring assignment or deadline
- A personal goal or habit being tracked

### Step 3 — Search for Specifics (Optional but Powerful)

If Step 1 reveals an active project or thread, use `conversation_search` to pull richer detail:

```
conversation_search(query="<active project keyword>", max_results=3)
```

Only do this if the recent_chats summary is too high-level to reconstruct meaningful context.
Use 1–2 searches max. Don't over-search.

### Step 4 — Synthesize and Present the Continuity Brief

Deliver a structured but conversational "catch-up" to the user. Format:

---

**📍 Where we left off**
[1–2 sentences on the most recent topic/task from the last session]

**🔄 Still in motion**
[Bullet list of active threads — things not yet finished or resolved]

**⏳ Waiting on / Next steps**
[Any items the user was going to do, check, or follow up on]

**💡 Ready when you are**
[Offer to dive into whichever thread they want to continue]

---

Keep this brief: 5–10 lines total. This is a briefing, not a report.
Do NOT list every single thing discussed — only what's *actionable* or *unresolved*.

### Step 5 — Hand Off Naturally

After the brief, ask one focused question to re-engage:
> "Want to pick up with [most active thread], or is there something new on your mind?"

Don't list options as a menu. Make it conversational.

---

## Tone and Behavior Rules

- **Don't sound like a robot reading logs.** Speak like a collaborator who remembers.
- **Don't announce the memory system.** Never say "I searched your past conversations." Just know.
- **Prioritize recency and urgency.** Surface what matters most, not everything.
- **Respect the user's pace.** If they just say "hi", don't dump a 20-item status list.
- **When in doubt, be brief.** A 5-line brief that's accurate beats a 20-line one that's noisy.
- **If nothing relevant is found**, just say: "Fresh start today — what are we working on?"

---

## Example Output

> Morning! Here's where things stand:
>
> **📍 Where we left off**
> We were finalizing your cold email to Prof. Ramakrishnan at IIT Mandi for the summer research
> internship — you had a draft ready but hadn't sent it yet.
>
> **🔄 Still in motion**
> - NITT Summer Programme applications: awaiting faculty consent emails before submitting the Google Form
> - Ostras V2 workflow: the receipt deduplication logic was in progress
> - BookFinder NLP: submitted, but you mentioned wanting to add a co-author attribution section
>
> **⏳ Next steps**
> You said you'd check for the NOC from EADA admin and circle back
>
> Want to pick up the IIT Mandi email, or is there something new pressing today?

---

## Edge Cases

| Situation | Behavior |
|---|---|
| No past chats found | "Looks like we're starting fresh — what are you working on?" |
| Last chat was > 7 days ago | Pull it anyway, but flag: "It's been a week — here's where we were:" |
| User asks for a specific topic | Skip the general brief, jump straight to `conversation_search` on that topic |
| User says "forget the past, new project" | Skip the skill entirely, start fresh |
| Memory already has rich context | Use memory first; only search chats if memory is insufficient |

---

## Notes

- This skill works best when combined with Claude's built-in memory (userMemories). Memory handles
  long-term facts about the user; this skill handles short-term, session-level recency.
- The skill is intentionally lightweight — it front-loads context retrieval so the rest of the
  session flows naturally, without the user needing to re-explain their situation every time.
- Maximum tool calls: `recent_chats` (1 call) + `conversation_search` (0–2 calls). Stay lean.
