# Notifications

**Status:** Draft · **Prefix:** NTFY

## Purpose

One inbox per user for everything requiring specs want to tell them, delivered in-app and by email.

## Requirements

| ID | Requirement | Verified by |
|---|---|---|
| NTFY-1 | Notification: `{id, user, type, subject {kind, id}, actor?, title (≤ 200), body? (≤ 2,000), count, created_at, updated_at, read_at?, archived_at?, snoozed_until?}`. `type` values and their recipients come from the requiring spec. | type unknown to the requiring spec → refused |
| NTFY-2 | At most one unread notification per `(user, type, subject)`: a new event updates it (`count + 1`, latest actor, `updated_at`) instead of adding another. | two comments on one item → one notification, count 2 |
| NTFY-3 | The actor is never notified about their own action. | user comments on own item → no notification |
| NTFY-4 | Inbox: newest `updated_at` first; filters `unread`, `archived`, `snoozed`; cursor pagination, 50 per page; unread count shown. | 120 unread → 3 pages, count 120 |
| NTFY-5 | Mark read or unread, mark all read, archive and unarchive; archived notifications leave the inbox but stay searchable. | mark all read → unread count 0 |
| NTFY-6 | Snooze until a time (in the user's `@specfork/auth:AUTH-1` time zone): hidden until then, then back on top as unread. | snooze to 09:00 tomorrow → reappears 09:00 unread |
| NTFY-7 | Preferences per `(type, channel)`, channels `in_app` and `email`; defaults set by the requiring spec; types it marks `required` can't be turned off in-app. | email off for type → no email, still in app |
| NTFY-8 | Email per type: `off`, `instant` (sent within 1 minute) or `daily` (one digest per local day at `digest_time`, default 08:00, skipped when nothing is new). | daily: 3 notifications → one email at 08:00 |
| NTFY-9 | Every email has a one-click unsubscribe (RFC 8058) that turns off email for that type only. | unsubscribe → that type email off, others on |
| NTFY-10 | A failed email is retried after 1, 5 and 30 minutes, then marked failed; in-app delivery never depends on email. | mail server down → 3 retries, still in app |
| NTFY-11 | Open sessions see new notifications and the unread count within 5 seconds, with no reload. | notification created → badge updates ≤ 5 s |
| NTFY-12 | Retention: read and archived notifications are deleted after 90 days, unread after 365. | read 91 days ago → gone |
| NTFY-13 | When the subject is deleted, its notifications are deleted; when a user loses access to it, they are hidden. | item deleted → its notifications gone |
| NTFY-14 | The inbox can be filtered by `type` and by a scope the requiring spec defines (e.g. project); unread counts are available per type and per scope. | filter project A → only A's notifications |
| NTFY-15 | Mark read for a subject: every notification of the user about that subject becomes read; requiring specs call it when the user opens the subject. | open item → its notifications read |

## Out of scope

- Mobile and web push, SMS, chat integrations: later channels.
- Per-subject subscribe/unsubscribe (watching): the requiring spec decides recipients.

## Decisions

- Grouping by `(user, type, subject)` keeps busy items from flooding the inbox.
- Email digests are per type so urgent types can stay instant.
