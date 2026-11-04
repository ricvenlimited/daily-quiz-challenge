# Daily Quiz & Challenge

A global quiz and learning product under the Ricven brand. V1 is the existing React/TypeScript/Vite/Capacitor application. V2 is the native Android product built with Kotlin and Jetpack Compose.

## Product and platform

- **V1:** existing production app; preserve its source, signing, Worker routes and release behavior.
- **V2:** native Android, developed on `v2-development`; do not merge into V1 `main` without explicit approval.
- **Backend:** Cloudflare Workers. Cloudflare is also the planned AI platform for V2 question generation.
- **V2 AI:** Workers AI is the first inference option to evaluate; AI Gateway/provider routing may be added where quality, reliability, observability or cost requires it. AI-generated questions must be grounded, validated and approved before serving.
- **Design:** Penpot is the active design workspace.

## Current V1 technology

React 19 · TypeScript 5.7 · Vite 6 · Capacitor Android 8.4.2 · Cloudflare Workers · IndexedDB · localStorage · AdMob.

Android application ID: `com.richard.dailyquizchallenge`.

## Cloudflare Worker

- Worker: `daily-quiz-intermidiary`
- Entry point: `worker/index.js`
- Base URL: `https://daily-quiz-intermidiary.richardoha25.workers.dev`

The V1 Worker integrates Open Trivia DB (General Knowledge and Science), Africa API (Africa & Nigeria), fact-first Current Affairs and a separate News Quiz route using NewsData.io. Current V1 configuration has no D1/KV/R2 binding. Do not infer that V2 database or AI services are already deployed.

Secrets such as `AFRICA_API_KEY` and `NEWSDATA_API_KEY` belong in Cloudflare secret configuration, never in source control. Keep News Quiz credentials and code separate from Current Affairs.

## Documentation — eight active documents

| Document | Authority |
|---|---|
| [project.md](project.md) | Product boundaries, V2 roadmap, stages, milestones and project-level open decisions. |
| [ui-ux-project.md](ui-ux-project.md) | Brand, design tokens, navigation UX, screens, components, accessibility and Penpot decisions. |
| [v2-native-architecture.md](v2-native-architecture.md) | Kotlin/Compose app architecture, client state, navigation implementation and Android integrations. |
| [backend-architecture.md](backend-architecture.md) | Cloudflare services, APIs, AI generation pipeline, security, scoring trust and operations. |
| [database-architecture-v2.md](database-architecture-v2.md) | V2 schema, question provenance, validation metadata, relationships and data integrity. |
| [current-affairs-project.md](current-affairs-project.md) | Current Affairs sources, fact model, freshness, question families and category-specific quality rules. |
| [commercial-monetization.md](commercial-monetization.md) | Ads, Premium, Store, billing, entitlements, licensing and AI-generation economics. |
| [README.md](README.md) | Repository overview and links to the authoritative documents. |

Each subject has one authority. The other documents reference it rather than duplicating its detailed specification. The overall production sequence exists only in `project.md`.

## Local V1 development

```bash
npm install
npm run dev
npm run build
npx cap sync android
npm run android:debug
```

The Android build requires a suitable Java and Android SDK environment.

## V2 product boundaries

V2's five primary destinations are Home, Explore, Challenges, Rankings and Profile. Authentication supports email/password and Google sign-in. Bible, News Quiz, monetization and other nested experiences remain subject to their respective design and product decisions. Features described as planned are not implemented until verified in code and deployment.
