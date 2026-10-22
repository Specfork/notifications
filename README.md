---
specfork: 1
name: "@specfork/notifications"
summary: Per-user notification inbox with read, archive and snooze, per-type channel preferences, email delivery and digests.
license: CC-BY-4.0
tags: [base, messaging]
requires:
  "@specfork/auth": "^0.3.0"
---

# Notifications

A base spec: requiring specs define notification types and who receives them; this spec stores, shows and delivers them.
Designed from patterns shared by Plane, Chatwoot, Vikunja and wger; no source text or code used.

| Area | Prefix | Covers |
|---|---|---|
| [notifications.md](specs/notifications.md) | NTFY | Inbox, grouping, read/archive/snooze, preferences, email, digests, retention |
