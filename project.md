# Daily Quiz & Challenge — Project Authority & Roadmap

**Status:** V2 documentation baseline established; Brand & Foundations is the next production milestone.
**Repository:** ricvenlimited/daily-quiz-challenge
**V1 branch:** main (stable; preserve)
**V2 branch:** v2-development
**Product:** Global quiz and learning app under the Ricven brand.

## 1. Purpose and authority

This is the **single authoritative source for the V2 project roadmap**: production stages, sequencing, dependencies, milestones, project-level status and project-wide open decisions.

It does not replace the specialist documents. Each specialist document owns the detailed decisions in its domain. Keep this file concise; do not copy full technical, design, database or commercial specifications into it.

## 2. Product and V1/V2 boundaries

- V1 is the existing React, TypeScript, Vite and Capacitor application on main. It is a stable reference, not the V2 implementation target.
- V2 is a native Android application built with Kotlin and Jetpack Compose in this existing repository on v2-development.
- Cloudflare is the primary backend and AI platform for Daily Quiz & Challenge. Firebase is not the V2 backend direction.
- AI-assisted question generation is a core V2 capability across categories and a major content-supply source, but it complements multiple external/curated knowledge sources rather than replacing factual source verification.
- Preserve V1 source, production Worker routes, provider integrations, AdMob configuration, signing identity and release behavior. Do not merge V2 into main or change V1 production behavior without explicit approval.
- The product is global. Do not introduce Africa- or Nigeria-only positioning as the overall product identity.
- V2's approved primary destinations are Home, Explore, Challenges, Rankings and Profile. Quiz, answer feedback, results, authentication, settings and Premium/Store are nested flows, not extra primary tabs.
- Design and prototype in Penpot before implementing the approved native Compose UI. Design account screens early; defer live authentication integration until the native UI foundation and screens are in place.
- Backend, billing and account integration must not be represented as implemented until code and deployment prove it.

## 3. V2 production roadmap — sole phase authority

The stages below govern the whole V2 production. No other document may create a competing project-wide phase sequence. Specialist documents may define technical checklists and acceptance criteria for their own domain, but must refer back here for project sequencing.

| Stage | Production outcome | Exit condition |
|---|---|---|
| 1. Architecture and documentation baseline | Product boundaries, specialist document ownership, native direction, backend/data boundaries and unresolved decisions are reviewed. | The documentation set is consistent, concise and has one authority per subject. |
| 2. Brand and visual foundations | Approved icon/mark direction, launch assets, colour and semantic tokens, typography, light/dark foundations and reusable design principles. | Brand and token decisions are reviewed and recorded in the UI/UX authority. |
| 3. Navigation and user journeys | Five-tab shell behavior, nested journeys, guest/account boundaries, quiz/challenge/ranking flows and screen inventory. | Product-level UX decisions are resolved or explicitly left as approved open items; journeys are mapped. |
| 4. Penpot design and prototype | Wireframes, high-fidelity light/dark screens, interaction states and a clickable prototype. | Major flows are reviewed on target Android screen sizes and approved before implementation. |
| 5. Native Android foundation | Kotlin/Compose project structure, build configuration, theme, reusable components, navigation shell and debug build. | A reproducible V2 debug build and baseline tests pass without disturbing V1. |
| 6. Native screen implementation | Approved screens and interactions, including quiz, results, profile, challenge, ranking, settings and account UI. | Required states, accessibility and navigation are validated on Android devices. |
| 7. Cloudflare backend and AI content platform | Versioned APIs, D1/persistence decision, Workers AI and AI Gateway evaluation, provider adapters, asynchronous generation/validation pipeline, question-bank ingestion, security and observability. | AI model/provider evaluation, cost/quality controls, source-to-question pipeline and backend contracts pass; V1 routes remain compatible. |
| 8. Authentication and account integration | Email/password and Google sign-in, verification/recovery, sessions, profile sync and account lifecycle. | End-to-end account flows and authorization tests pass. |
| 9. Quiz, learning, challenges and rankings | Integrate the approved question bank and AI-assisted content pipeline into quiz delivery, with trusted sessions/scoring, results/history, progress, challenges, achievements and non-cash rankings. | Questions are sourced, validated, deduplicated, versioned and served safely; server-authoritative behavior and abuse controls pass end-to-end tests. |
| 10. Monetization, stabilization and release | Native ads, approved billing/entitlements, security/privacy/accessibility/performance testing and release preparation. | Release and migration checks pass; production rollout requires explicit approval. |

**Sequencing rules**
- Stages 2–6 are design-first: do not integrate live authentication before the native UI is built.
- Backend discovery and contract planning may proceed as dependencies require, but backend implementation must not displace the approved design-first sequence.
- Ads, billing and entitlements must be accounted for in UX and architecture early; implementation belongs in Stage 10 unless a specific dependency is approved earlier.
- Stage completion requires evidence and review; a document proposal alone is not proof of implementation.

## 4. Current project status

- **Stage 1 — complete.** The eight-document authority map and V2 roadmap have been reconciled; the AI question-generation requirement is included.
- **Next: Stage 2 — Brand and visual foundations.**
- V2 application implementation, D1 deployment, authentication services, billing and native ads are not claimed as complete.
- Penpot is the active design workspace. Figma is reference/backup only.
- V1 remains the production baseline on main.

## 5. Project-level decisions still open

These are intentionally unresolved. Do not silently decide them in implementation.

1. Guest access: whether guests can play quizzes, and which actions require sign-in.
2. Bible entry point: whether Bible lives within Explore or has another nested entry.
3. Daily Challenge: how the Home card relates to the Challenges destination and its daily challenge lifecycle.
4. News Quiz: whether and where this separate live-news product appears in V2.
5. Quiz navigation: whether the five-item bottom bar is hidden during active quiz play (current UX proposal: hide it).
6. Rankings: global/country/friends scope, eligibility, and what profile information is publicly displayed.
7. Premium/Store: exact placement and entry points; it must not become a sixth primary tab.
8. Typography: final typeface and eight-level type hierarchy.
9. Semantic tokens: warning, disabled and dark-mode success text colours; verify all text contrast combinations.
10. Brand intro: whether a first-launch brand intro is needed in addition to the system splash and optional tutorial.
11. AI implementation choices: initial Workers AI model(s), whether/when to add an external model through AI Gateway, evaluation thresholds, human-review scope and generation budgets. AI itself is required; these implementation details remain open.

The UI/UX document owns the detailed experience decisions. Backend/database documents own the security, data and access implications. This register tracks only whether a project-level decision remains open.

## 6. Authoritative document map

| Document | Sole authority |
|---|---|
| README.md | Short repository introduction, setup and links to the active documents. |
| project.md | Product scope, V1/V2 boundaries, production roadmap, milestones and project-level open decisions. |
| ui-ux-project.md | Brand, visual system, navigation UX, screen inventory, components, interaction states, accessibility and Penpot design decisions. |
| v2-native-architecture.md | Kotlin/Compose application architecture, Android modules/layers, client state, navigation implementation and native integration boundaries. |
| backend-architecture.md | Existing Worker facts, Cloudflare services, APIs, provider boundaries, AI inference/generation services, backend security, scoring trust boundary and operations. |
| current-affairs-project.md | Current Affairs domain coverage, source registry, verified fact model, freshness rules, question families and category-specific content validation. |
| database-architecture-v2.md | V2 data model, entities, relationships, constraints, indexes, lifecycle and data integrity. |
| commercial-monetization.md | Ads, Premium, Store, billing, pricing, entitlements and commercial policy. |

`current-affairs-project.md` remains an active specialist authority for Current Affairs sources, fact integrity, freshness, question families and category-specific content quality. It does not own the overall V2 roadmap or generic Cloudflare AI infrastructure.

## 7. AI and question-content strategy

AI-assisted question generation is a core capability for V2 and must support all appropriate categories. The long-term content goal is a very large, diverse question supply (potentially over one million records), built progressively and only where quality and rights permit.

Use a multi-source pipeline:

**Knowledge providers / curated facts → normalized source records → AI-assisted question drafts → factual and structural validation → duplicate/family checks → approved question bank → quiz serving.**

- AI is a major generation and enrichment source, not the sole authority for truth.
- Preserve source provenance, fact/concept identity, question-family identity, difficulty, explanation, freshness, content version, validation status and generation/model metadata.
- Use Workers AI as the first Cloudflare-native inference candidate. Evaluate models rather than locking the product to one model. AI Gateway provides a route to monitor, cache, rate-limit and add other providers if quality, reliability or cost requires it.
- Generate most content asynchronously in controlled batches; do not make each player's quiz wait for bulk generation. Runtime generation may be a bounded recovery path only after the same validation gates.
- Generated questions must never enter the serving bank solely because a model returned valid JSON. Factual checks, schema checks, answer/distractor checks, duplicate detection and category-specific freshness/rights rules are required.
- Keep question-generation prompts, model credentials and provider keys on the backend. The Android app consumes approved questions through versioned APIs.
- Select models, limits, review thresholds and generation budgets during Stage 7. Integrate the serving pipeline into quiz flows during Stage 9.

The backend document owns the generic AI service/pipeline design; current-affairs-project.md owns Current Affairs-specific factual source and generation rules; the database document owns stored question and provenance fields; the commercial document owns AI cost controls. This section records the project-level commitment only.

## 8. Current Affairs and News Quiz boundary

- Current Affairs is a fact-first verified knowledge system with provenance, freshness and question-family/anti-repetition controls.
- News Quiz / Current Events is a separate live-news product and provider path. NewsData-specific credentials, parsing and generation must remain isolated.
- The Current Affairs Question Bank and local recent-question history are separate systems.
- current-affairs-project.md owns Current Affairs content/source rules; backend-architecture.md owns the technical implementation boundary. This project document records only the product distinction.

## 9. Documentation change control

- Keep each decision in its authoritative document. Other documents may link to it and state only the minimum dependency needed for their own domain.
- Do not duplicate the project roadmap, design tokens, database schema, backend security design or commercial model across documents.
- Keep documents concise. Remove superseded instructions and repeated status logs when their durable decisions are captured in the current authority.
- Label facts, approved decisions, proposals and unresolved questions distinctly.
- Update the project status and roadmap here when a stage changes; update the relevant specialist document only when its domain specification changes.
- Work directly on v2-development for documentation-only changes when requested. Do not create a branch, PR or merge unless explicitly requested.
- Never modify main or V1 production behavior as part of V2 documentation work.
