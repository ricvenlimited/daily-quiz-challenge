# Daily Quiz & Challenge

A global quiz and learning app built with React, TypeScript, Vite and Capacitor for Android. The current V1 application is the functional reference while the next major product redesign is planned separately.

## Product

Daily Quiz & Challenge is developed under the Ricven brand. Its product direction combines daily quizzes, multiple knowledge categories, engaging gameplay, achievements and a longer-term learning/performance experience.

## Current technology

- React 19
- TypeScript 5.7
- Vite 6
- Capacitor Android 8.4.2
- Cloudflare Workers backend intermediary
- IndexedDB for local recent-question history
- localStorage for lightweight local streak/best-score state
- AdMob for the current V1 advertising integration

Android application ID: `com.richard.dailyquizchallenge`  
Package version in `package.json`: `1.1.9`.

## Backend

Worker configuration: `wrangler.toml`

- Worker name: `daily-quiz-intermidiary`
- Entry point: `worker/index.js`
- Base URL: `https://daily-quiz-intermidiary.richardoha25.workers.dev`

The Worker integrates Open Trivia DB (General Knowledge and Science), Africa API (Africa & Nigeria), the fact-first Current Affairs implementation and a separate News Quiz route using NewsData.io.

**Cloudflare Workers are the backend platform for this product.** No Cloudflare D1/KV/R2 database binding is currently configured. Current Affairs seed data is version-controlled JavaScript and runtime fallback generation is request-scoped.

See [backend-architecture.md](backend-architecture.md) for the verified architecture, security boundaries, current limitations and future hardening plan.

## Documentation

- [Project roadmap and continuity record](project.md)
- [UI/UX blueprint](ui-ux-project.md)
- [Backend architecture and security](backend-architecture.md)
- [Commercial and monetization plan](commercial-monetization.md)

The active design workflow uses **Penpot** as the design/prototyping workspace and **Claude** as an AI-assisted collaborator. Approved decisions must be recorded in project documentation.

## Local development

```bash
npm install
npm run dev
npm run build
npx cap sync android
npm run android:debug
```

The Android build requires the appropriate Java/Android SDK environment.

## Cloudflare secrets

The Worker expects secrets declared in `wrangler.toml`, including:
- `AFRICA_API_KEY`
- `NEWSDATA_API_KEY` (separate News Quiz provider path)

Configure secret values through Cloudflare's secret-management workflow. Never commit secret values, API tokens, signing keystores or passwords.

## Product boundaries

- The current React/Vite/Capacitor application is the V1 reference; future native redesign plans are not completed implementation.
- Bible remains a future product experience/V1 placeholder.
- Google Play Billing, subscriptions, Remove Ads purchases and premium entitlement enforcement are planned, not implemented.
- Preserve existing production signing and AdMob configuration during Android build changes.
