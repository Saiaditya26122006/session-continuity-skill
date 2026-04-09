# 🧠 Session Continuity Skill for Claude

> **Give Claude persistent memory across sessions — so every conversation picks up exactly where you left off.**

![Claude Skill](https://img.shields.io/badge/Claude-Skill-orange?style=flat-square&logo=anthropic)
![Version](https://img.shields.io/badge/version-1.0.0-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![Platform](https://img.shields.io/badge/platform-Claude.ai-purple?style=flat-square)

---

## The Problem

Every time you open a new Claude chat, you start from zero.

You have to re-explain your project, your context, what you were working on, what decisions you made. It breaks flow. It wastes time. And it makes Claude feel like a tool instead of a collaborator.

## The Solution

This skill turns Claude into a persistent collaborator. The moment you say **"hi"**, **"let's continue"**, or even just open a new chat — Claude automatically pulls your recent conversations, identifies what's unresolved, and delivers a structured briefing:

```
📍 Where we left off
We were finalizing the cold email to Prof. Ramakrishnan — draft was ready but not sent.

🔄 Still in motion
- Research internship applications: 3 pending faculty replies
- Automation workflow: deduplication logic in progress
- Report: submitted, but co-author section flagged for revision

⏳ Next steps
You mentioned checking the NOC document and circling back.

Want to pick up the internship emails, or is there something new today?
```

No re-explaining. No context-dumping. Just continuation.

---

## Demo

| Without this skill | With this skill |
|---|---|
| "Hi" → "Hello! How can I help you today?" | "Hi" → Instant briefing of your active threads |
| You re-explain your project every session | Claude already knows where you left off |
| Context lost after every chat | Continuity across days, weeks, projects |

---

## Installation

### Step 1 — Download the skill file

Download [`session-continuity.skill`](./skill/session-continuity.skill) from this repo.

### Step 2 — Open Claude.ai Settings

Go to [claude.ai](https://claude.ai) → Click your profile → **Settings**

### Step 3 — Upload the skill

Navigate to the **Skills** section → Click **Upload Skill** → Select the downloaded `.skill` file.

### Step 4 — Start chatting

Open a new conversation and just say **"hi"**. Claude will do the rest.

---

## How It Works

The skill uses Claude's built-in conversation tools:

1. **`recent_chats`** — Fetches your 5 most recent conversations
2. Scans for **active threads** (unresolved tasks, open questions, pending actions)
3. Optionally runs **`conversation_search`** to pull deeper context on specific projects
4. Synthesizes everything into a **5–10 line briefing** — brief, actionable, human
5. Hands off with one focused question to re-engage naturally

**Tool call budget:** Maximum 3 calls per session start (lean by design).

---

## Trigger Phrases

The skill activates automatically on session-opening messages like:

| Category | Examples |
|---|---|
| Simple greetings | "hi", "hey", "good morning", "hello Claude" |
| Continuation signals | "let's continue", "pick up where we left off", "where were we" |
| Status checks | "what's my current status", "catch me up", "what am I working on" |
| Vague references | "that thing we were doing", "the project", "the email we drafted" |

It does **not** trigger on specific, context-complete questions like "Explain binary search trees."

---

## Works Best With

- **Claude's built-in Memory** — Long-term facts (your name, role, preferences) live in Memory. This skill handles short-term, session-level recency. Together they cover both.
- **Power users with multiple active projects** — The more you use Claude, the more valuable this becomes.
- **Researchers, students, builders, founders** — Anyone who uses Claude daily for ongoing work.

---

## Edge Cases

| Situation | Behavior |
|---|---|
| No past chats found | "Looks like we're starting fresh — what are we working on?" |
| Last chat was 7+ days ago | Surfaces it anyway with a time flag |
| User asks for a specific topic | Skips general brief, searches that topic directly |
| User says "new project, forget the past" | Gracefully skips, starts fresh |

---

## File Structure

```
session-continuity-skill/
├── README.md                  ← You are here
├── skill/
│   ├── session-continuity.skill   ← Install this in Claude.ai
│   └── SKILL.md                   ← Raw skill instructions (human-readable)
├── CONTRIBUTING.md            ← How to improve this skill
├── CHANGELOG.md               ← Version history
└── LICENSE                    ← MIT
```

---

## Contributing

Found a bug? Have an improvement? PRs are welcome.

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

**Ideas for future versions:**
- `v1.1` — Priority scoring (surface most urgent thread first based on deadlines)
- `v1.2` — Weekly digest mode ("summarize my week")
- `v1.3` — Project tagging (group threads by project name)
- `v2.0` — Cross-device sync with external notes (Notion, Obsidian)

---

## Author

**Sai Aditya** — AI & Automation Builder

- 🎓 Double Degree: Global BBA + AI for Business @ SKEMA & EADA Business School, Barcelona
- 🤖 AI Workflow Automation Intern @ Ostras Descuentos
- 🏛️ Founder, AI & Business Club @ EADA

Built this because I got tired of re-explaining my projects to Claude every single morning.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/sai-aditya-talluri-03ab8b2b0)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat-square&logo=github)](https://github.com/Saiaditya26122006)

---

## License

MIT — free to use, modify, and distribute. Attribution appreciated.

---

*If this saves you time, consider starring the repo ⭐ — it helps others find it.*
