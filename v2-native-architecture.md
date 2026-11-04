# Daily Quiz & Challenge — Native Android V2 Architecture

**Status:** Approved target architecture; implementation is not yet claimed complete.
**Platform:** Native Android · Kotlin · Jetpack Compose
**Project roadmap:** See [project.md](project.md). This document does not own production phases.

## 1. Scope and branch boundaries

This document is the authoritative source for the Android client's technical architecture: app structure, Compose implementation, state, navigation implementation, local client data and integration boundaries.

- Repository: ricvenlimited/daily-quiz-challenge.
- V1 stable branch: main. V2 working branch: v2-development.
- V1 remains the existing React/TypeScript/Vite/Capacitor app. Preserve its code, application identity, signing, AdMob and production behavior.
- V2 is a native Android client in the existing repository. Do not copy the V1 web UI into the native app.
- Cloudflare is the backend platform; backend service design belongs to backend-architecture.md.
- Data entities and persistence schema belong to database-architecture-v2.md.
- Brand, screen appearance and UX behavior belong to ui-ux-project.md.
- Commercial behavior belongs to commercial-monetization.md.

Do not merge V2 into main or alter V1 production behavior without explicit approval.

## 2. Confirmed Android product constraints

- Native client: Kotlin and Jetpack Compose with Material 3 customized through approved Ricven design tokens.
- Five primary destinations: Home, Explore, Challenges, Rankings and Profile. The UI/UX document owns their product meaning and detailed behavior.
- Quiz, answer feedback, results, authentication, settings, Premium/Store and other detail experiences are nested flows, not extra primary destinations.
- Authentication methods: email/password and Google sign-in. Design the account screens with the UI; integrate live authentication only after the native UI foundation and screens are ready.
- Penpot is the active design source. Implement only approved designs.
- V2 must preserve V1 and use separately versioned backend contracts.

## 3. Approved Android technology direction

| Concern | Direction |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose |
| Design foundation | Material 3 + approved app theme/tokens |
| Navigation | Navigation Compose |
| Presentation architecture | MVVM with clear UI, domain and data boundaries |
| UI state | ViewModel + StateFlow; immutable screen state |
| Async work | Kotlin Coroutines and Flow |
| Networking | Choose Retrofit or Ktor during implementation; use one consistent HTTP client |
| Local relational data | Room only where a demonstrated offline/cache need justifies it |
| Preferences | DataStore |
| Dependency injection | Select one approach before broad implementation |
| Build | Gradle with centralized dependency/version management |
| Tests | Kotlin unit, Compose UI, API/repository contract and Android instrumentation tests |
| CI | GitHub Actions with a reproducible V2 debug build |

Do not introduce competing HTTP clients, multiple DI frameworks or redundant persistence libraries. Record exact dependency versions when implementation begins and verify current Android compatibility.

## 4. Client architecture and responsibilities

Recommended dependency direction:

```text
Compose UI / Navigation
        ↓
ViewModel (StateFlow)
        ↓
Domain use cases
        ↓
Repository interfaces
        ↓
Data implementations
   ├── Cloudflare API client
   ├── Room (only when justified)
   └── DataStore
```

- Compose screens render state and dispatch user actions. They do not own business rules or issue ad-hoc network requests.
- ViewModels coordinate screen state and UI events.
- Domain use cases contain reusable client-side application rules; they must not be treated as trusted scoring authority.
- Repositories isolate remote and local data sources.
- Keep navigation destinations and feature modules cohesive. Avoid premature module fragmentation; introduce modules when ownership or build/test boundaries justify them.
- Keep credentials and session material out of logs, ordinary preferences and source control. Select secure storage based on the final session design.

## 5. Navigation and lifecycle implementation

Implement the approved five-destination shell using Navigation Compose. The UI/UX authority defines the information architecture and interaction rules; this document defines their Android implementation.

- Use nested navigation for detail and task flows.
- Preserve appropriate tab state/back stacks when switching destinations.
- Define Back/Up, deep-link and process-restoration behavior.
- Preserve quiz progress safely across configuration changes and process recreation where required.
- Handle authentication gates based on backend authorization, not client-only flags.
- Do not decide unresolved UX questions (such as whether the bottom bar is hidden during an active quiz) in this architecture document.

## 6. Backend and data integration boundaries

The Android client communicates with Cloudflare through documented, versioned HTTPS APIs. It must not connect directly to D1 or call credentialed third-party providers from the app.

- API routes, request/response contracts, provider adapters, backend security and server-authoritative scoring are defined in backend-architecture.md.
- Tables, relationships, indexes, migrations, retention and data integrity are defined in database-architecture-v2.md.
- The client never supplies authoritative score totals, correctness flags, premium status or user ownership claims.
- Question delivery must not expose trusted answer keys. The Worker evaluates submitted answers and returns permitted feedback.
- Use explicit loading, empty, error, offline and retry states. Network failure must not block access to screens or local content that is designed to remain available.
- Keep V1 API contracts compatible while V2 uses its separately versioned API surface.

## 7. Quality, accessibility and reliability

- Use the UI/UX authority for required visual and interaction states, contrast, type scaling, touch targets, motion and light/dark behavior.
- Keep answer feedback understandable without colour alone.
- Handle lifecycle changes, connectivity loss, timeouts, retries and expired sessions predictably.
- Avoid blocking Home on authentication, remote configuration, ads or backend initialization.
- Test navigation, state restoration, offline behavior, screen sizes, accessibility and real-device performance.
- A reproducible debug APK and passing baseline tests are required before broad feature implementation.

## 8. V1 compatibility and migration

- Keep V1 operational on main throughout V2 development.
- Do not assume V1 IndexedDB/localStorage values are trusted account records or safely migratable.
- Decide whether any legacy local data can be imported only after its integrity and ownership are reviewed.
- Preserve the existing application ID and signing considerations until package identity, signing continuity, data migration and in-place upgrade compatibility are verified. Do not change package identity by assumption.
- Evaluate native AdMob integration separately from the existing V1 Capacitor AdMob implementation. The commercial plan belongs to commercial-monetization.md and V2 native ad implementation is governed by project.md Stage 10.
- Do not remove V1 files or change production Worker behavior as a side effect of Android foundation work.

## 9. Change control

- Keep Android-specific implementation decisions here; link to the other authoritative documents for their domains.
- Do not reproduce the project-wide production roadmap in this file.
- Mark technical choices as approved, proposed or unresolved.
- Update this document when Android stack, app structure, client state or integration behavior changes.
- Work directly on v2-development for documentation-only changes when requested. Do not create a branch, PR or merge without explicit approval.
