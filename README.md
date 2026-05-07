# Offline App Architecture Notes

Architecture notes for designing local-first desktop applications with reliable data storage, traceability and audit-ready workflows.

This repository collects generic principles for offline-first internal tools, especially in field-service, industrial and operational environments.

## Purpose

Many internal tools are designed as if stable network access, centralized systems and perfect user behavior are always available.

In real operational environments, this is often not the case.

Offline-first architecture helps ensure that critical workflows remain usable even when network access is unavailable, unstable or intentionally avoided.

## Core architecture idea

A local-first desktop app should treat the local database as the primary operational source of truth.

External synchronization, cloud storage or central reporting can be added later, but the core workflow should not depend on permanent connectivity.

## Typical stack

```text
Desktop shell: Tauri
Frontend: Next.js / React / TypeScript
Local database: SQLite
Styling: Tailwind CSS
Testing: Playwright
Audit concept: structured change history
```

## Key design principles

### 1. Local database as primary source

The application should store operational data in a defined local database.

Important considerations:

- use a predictable storage path
- separate business data from runtime cache
- define migration behavior
- prevent uncontrolled overwrites
- protect against accidental data loss

### 2. Runtime cache is not business data

Desktop apps may create runtime folders for WebView data, cache, certificates, GPU files or temporary browser artifacts.

These files should not be treated as the authoritative business database.

A clean architecture separates:

```text
Business data → local database
Runtime data  → cache / WebView / temporary files
Config data   → controlled configuration files
```

### 3. Audit log by design

Audit-relevant workflows should record important changes.

A useful audit log should answer:

- who changed something?
- what was changed?
- when was it changed?
- which entity was affected?
- what was the previous value?
- what is the new value?
- on which device or installation did it happen?

### 4. Status changes need history

For operational tools, the current status alone is not enough.

The system should also preserve the status path.

Example:

```text
Available
   ↓
Issued
   ↓
Returned
   ↓
In inspection
   ↓
Available or Blocked
```

This makes responsibility and process quality easier to review.

### 5. Offline-first is a product decision

Offline-first is not only a technical implementation detail.

It affects:

- user workflow
- data ownership
- error handling
- backup strategy
- synchronization roadmap
- auditability
- IT acceptance

## Recommended data model concepts

Useful entities for local-first operational tools:

- users
- devices
- app installations
- equipment or workflow items
- transactions
- audit log entries
- configuration records
- migration metadata

## Risk areas

Common risks in offline-first apps:

- duplicate records after import/export
- unclear source of truth
- missing backup process
- hidden data in runtime folders
- weak audit history
- inconsistent status logic
- uncontrolled multi-device usage
- unclear migration behavior

## Practical recommendation

Start with a controlled single-device pilot before introducing multi-device workflows.

A single-device pilot is easier to validate, audit and explain.

Multi-device usage should only be introduced with a clear synchronization and conflict strategy.

## Principle

```text
Offline-first is not a fallback.
It is an architecture decision for unreliable real-world environments.
```

## Status

```text
Public architecture notes · Generic content · No private source code
```

## Notes

This repository contains public, generic architecture notes.

It does not include private implementation details, company data, credentials, production databases, customer data or internal configuration files.
