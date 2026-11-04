# Daily Quiz & Challenge — UI/UX Project Blueprint
## Current Affairs product-architecture checkpoint — 23 September 2026

The future Current Affairs experience is broader than live news. It will cover structured current/general knowledge across **Nigeria, Africa, World, International Organizations, Economy & Business, Geography, Sports, and Science & Technology**. The current V1 UI should continue showing a single Current Affairs category card; the future native redesign may expose these domains/subcategories after the content system is stable.

A separate future **News Quiz / Current Events** experience may use NewsData.io or another news provider for actual recent-news questions. It must remain a separate product experience and provider path rather than being mixed into Current Affairs.


**File purpose:** Dedicated design workspace for the future UI/UX redesign.  
**Important:** This file is separate from the main `project.md`. The main `project.md` remains the concise source of truth for app development status.

## 1. Design Project Status

**Current phase:** V2 brand and native UI/UX design planning. V1 is stabilized and remains the reference implementation; the full redesign belongs to the native Kotlin/Jetpack Compose application on `v2-development`, not the React/Vite/Capacitor V1 codebase.

The approved next milestone is to finalize the Royal Navy-led brand identity, app icon/adaptive icon and splash direction; complete the light/dark design system and restrained premium background language; map navigation and user journeys; and design the complete screen inventory in Penpot. Sign-in and registration screens are included in this early design work, while real email/password and Google authentication integration follows the native UI foundation. Keep this file synchronized with `project.md` and `v2-native-architecture.md` at major checkpoints.

This file is the primary UI/UX blueprint for the native V2 redesign. Historical V1 checkpoints below are retained for continuity and do not override the current V2 direction.

### V1 checkpoint — 22 September 2026
The redesign is intentionally deferred while the current V1 is completed. The current React/Vite/Capacitor UI remains the working validation interface. Discussion of a future native Android foundation, new product/company branding, and expanded learning-platform navigation is planning only; none should trigger a production UI rewrite yet. After Bible and Current Affairs are working, a full V1 stability pass will precede the major UI/UX implementation.

## 2. Product Vision

**Product:** Daily Quiz & Challenge  
**Developer/brand:** Ricven

Long-term goal: build a polished, dependable quiz platform that people enjoy returning to, with fresh questions, strong category experiences, engaging gameplay, useful results, dependable monetization, and a professional visual identity.

The redesign should make the product feel like a **premium modern quiz/game experience**, not a beginner project, while remaining fast, clear, accessible, and scalable.

## 3. Core Design Concept

### Daily Quiz & Challenge — Clean Competitive

Target feeling:

- Clean
- Modern
- Premium
- Energetic
- Competitive
- Playful without being childish
- Sharp and crisp
- Easy to understand
- Similar in polish to a modern fintech/gift-card app, but adapted to a quiz/game experience

Avoid:

- Excessive gradients
- Visual clutter
- Generic/basic-looking layouts
- Overly corporate styling
- Overly childish game styling
- Unnecessary decoration
- Making every screen look different

The app should have one coherent design language.

## 4. Inspiration

Use external UI/UX work as **research and inspiration, not templates to copy**.

Primary inspiration reviewed:

- Behance quiz-app UI/UX projects and search results
- The referenced Behance project: “Quizzing game ui ux design”
- Other modern quiz interfaces emphasizing clean question layouts, dashboards, statistics, badges, results, and gamification

Reference:
https://www.behance.net/gallery/146388163/Quizzing-game-ui-ux-design

What we are studying from references:

- Layout hierarchy
- Card composition
- Spacing
- Typography
- Question presentation
- Answer interaction
- Progress indicators
- Timers
- Dashboards
- Results screens
- Streaks and achievements
- Visual depth
- Navigation
- Gamification
- Overall polish

Create an original Daily Quiz & Challenge expression of the Ricven identity using the shared Ricven/RichInsights brand system.

## 5. Approved Ricven / RichInsights Brand Colour System

**Source of truth:** the current RichInsights Android implementation, `app/src/main/java/com/ricven/richinsights/ui/theme/Color.kt` (Colours v2). Daily Quiz & Challenge must reuse these exact shared brand tokens. This replaces the experimental palette in the original draft.

### Brand primitives

| Token | Hex | Intended use |
|---|---|---|
| Royal Navy | `#0B2970` | Brand anchor and strong surfaces |
| Blue | `#1769E0` | Primary actions and selected navigation |
| Electric Cyan | `#19B5FE` | Progress, focus and signature accents |
| Warm Gold | `#F4B942` | Achievements, trophies and meaningful milestones; use sparingly |

### Light surfaces and text

| Token | Hex |
|---|---|
| Surface | `#FFFFFF` |
| Surface Accent | `#E8F7FF` |
| Surface Strong | `#0B2970` |
| Outline | `#D9E2F2` |
| Progress Track | `#E3EAF7` |
| Primary Text | `#0D0D0D` |
| Body Text | `#2C2C2C` |
| Secondary Text | `#565656` |

### Dark surfaces and text

| Token | Hex |
|---|---|
| Background | `#061332` |
| Surface | `#0D1F49` |
| Outline | `#1C3672` |
| On Strong Surface | `#FFFFFF` |
| On Strong Body | `#E0E2E7` |
| On Strong Secondary | `#B6BCC8` |

### Supporting semantic colours

| Token | Hex |
|---|---|
| Success | `#22A06B` |
| Success Text | `#167A4C` |
| Error | `#C93B3B` |
| Dark Error | `#FFB4AB` |

### Usage and accessibility rules

- Keep the brand primitives consistent in light and dark themes; adapt surfaces and text using the tokens above.
- Gold is an achievement/reward accent, not a routine navigation or generic action colour.
- Cyan and Blue carry interaction, selection and progress emphasis. Avoid making every element bright.
- Never rely on colour alone for correct/incorrect states; pair colour with iconography and explicit labels.
- Validate contrast, text scaling and touch targets on actual Android screen sizes.
- The former experimental palette is superseded and must not override these exact shared brand tokens.

## 6. Typography

Typography must feel:

- Modern
- Highly readable
- Strong on headings
- Clear on quiz questions
- Comfortable on smaller phone screens
- Consistent across all screens

Typography selection and exact type scale will be finalized in the V2 visual-identity phase and documented as reusable native theme tokens.

Typography hierarchy should distinguish:

1. Screen titles
2. Major numbers/scores
3. Quiz questions
4. Section headings
5. Body text
6. Secondary/helper text
7. Button labels
8. Status/metadata

## 7. Layout Principles

The redesign should prioritize:

- Strong visual hierarchy
- Generous but efficient spacing
- Clear grouping
- Consistent margins/padding
- Comfortable touch targets
- Crisp alignment
- Limited visual noise
- Predictable interaction patterns
- Mobile-first layouts for the Oppo A56 and similar Android phones

Cards should have subtle depth rather than heavy shadows.

Use restrained background treatments to give selected screens a premium, polished sense of depth: soft ambient glows, very subtle tonal gradients, low-contrast abstract/geometric forms, gentle edge lighting and carefully controlled shadows. These may sit behind a full screen or within selected hero/header/card regions, but must remain secondary to content. Use low opacity, generous negative space and the Royal Navy/Blue/Cyan palette; in light mode keep surfaces bright and clean, and in dark mode use restrained cyan/blue ambient light rather than harsh neon. Avoid busy patterns, strong blur everywhere, multiple competing gradients, glass effects used indiscriminately, heavy drop shadows or decorative elements that look childish. Background effects must not impair contrast, text readability, battery/performance, scrolling or accessibility.

Corner radius, spacing scale, shadow/elevation levels, background treatment tokens and component dimensions will be standardized in the V2 design-system phase.

## 8. Home Screen Direction

The home screen becomes the main product experience.

Proposed hierarchy:

1. Greeting
2. Daily Challenge
3. Streak
4. Explore Categories
5. Additional features
6. Navigation

Concept:

**Good evening, Richard 👋**  
**Ready for today's quiz?**

Then a prominent Daily Challenge card containing:

- Daily Challenge label
- Short motivational message
- Current streak
- Play Now action

Then category exploration.

The home screen should not simply present five equal category rectangles.

## 9. Category System

Every category gets its own identity while remaining part of one design system.

The category model is now broader than the original five-card quiz layout. Bible is a multi-screen reading/study experience, while Current Affairs and Africa & Nigeria require online content workflows. The final navigation must accommodate these differences without fragmenting the overall design language.

Initial categories:

- General Knowledge — brain/globe/knowledge imagery
- Science — atom/science imagery
- Bible — book/light imagery; opens into a dedicated Bible experience rather than immediately forcing a quiz
- Africa & Nigeria — subtle African geographic/cultural identity
- Current Affairs — news/world imagery and freshness/status cues

Rules:

- Category imagery should support recognition.
- Avoid visually noisy cards.
- Typography/layout should remain consistent.
- Category-specific colours may be explored later, but the overall brand must remain coherent.

Future category additions must fit the same system.

## 10. Planned Screens

The redesign is a complete product experience, not just a prettier home screen.

Core screens:

1. Splash
2. Home
3. Category selection
4. Category configuration
5. Pre-quiz/ad transition
6. Quiz
7. Answer feedback
8. Results
9. Streak
10. Profile/statistics
11. Settings
12. About
13. Loading state
14. Error state
15. Offline/network state

Additional screens may be added as the product evolves.

## 11. Quiz Screen — Highest Priority

The quiz screen is the most important gameplay screen because users spend most of their time there.

Target structure:

- Back/navigation control where appropriate
- Question number, e.g. “Question 4 of 10”
- Progress indicator
- Timer
- Question
- Four answer options
- Streak indicator
- Clear answer feedback

Example information hierarchy:

Question 4 of 10  
[progress]

00:11

Which planet is known as the Red Planet?

A — Venus  
B — Mars ✓  
C — Jupiter  
D — Mercury

🔥 STREAK ×3

The exact visual composition will be designed during wireframing.

## 12. Timer

Current gameplay uses a 15-second countdown.

The redesign should make the timer part of the visual language without becoming distracting.

Potential progression:

15 → 12 → 9 → 6 → 3

Possible visual feedback:

- Subtle progress change
- Gentle urgency near the end
- Clear final countdown
- No excessive animation

Do not change underlying quiz timing merely for visual reasons without separately validating gameplay.

## 13. Answer Option States

Every answer component must have a defined state.

Required states:

1. Default
2. Pressed
3. Selected
4. Correct
5. Incorrect
6. Disabled/locked

Concept:

**Default:** white card + subtle border/depth  
**Pressed:** immediate tactile visual response  
**Selected:** blue emphasis  
**Correct:** green + checkmark  
**Incorrect:** red + X  
**Disabled:** muted after answer is locked

Answer feedback must remain understandable without relying on colour alone.

## 14. Results Screen

The results screen should become one of the most satisfying parts of the product.

Target information:

- Quiz complete
- Score
- Accuracy
- Points earned
- Streak
- Correct count
- Wrong count
- Progress visualization
- Next actions

Concept:

🎉

QUIZ COMPLETE

8 / 10

[progress]

+80 POINTS

🔥 4 DAY STREAK

Accuracy 80%  
Correct 8  
Wrong 2

[ PLAY AGAIN ]  
[ OTHER CATEGORY ]

Future enhancement:

- Question explanations
- Learning feedback
- Personalized performance information
- Achievement/reward feedback

Performance messaging can eventually vary appropriately, e.g. encouragement after a strong result or a supportive message after a difficult result.

## 15. Streaks, Points & Achievements

These should feel like part of the product identity.

Use the amber/gold accent selectively for:

- 🔥 Streaks
- 🏆 Achievements
- ⭐ Points
- 🎁 Rewards

These elements should feel rewarding without overwhelming the main quiz experience.

## V1.1.4 production checkpoint — 22 September 2026

Africa & Nigeria is now a real production content experience behind the Cloudflare Worker. The redesign must support source/attribution information for externally sourced content without cluttering the quiz screen. The existing AdMob architecture is considered validated and should be preserved during the redesign; production builds now select real AdMob IDs while Debug builds continue to use test ads.

## 16. Ads & Monetization UX

AdMob is already a functioning part of the current app and must be considered during redesign.

Principle:

**Ads should be integrated into the UX, not feel like accidental interruptions.**

Potential placements:

- Persistent banner area where appropriate
- Natural transition points
- Interstitial between suitable gameplay stages
- Rewarded ad as an explicit optional action
- App Open without destroying user context

Rewarded example:

**Watch an ad → +20 points**

The user should understand that the rewarded action is optional.

The redesign must preserve ad functionality and must not hide, overlap, or break the ad containers.

## 16.1 Billing, Premium and purchase UX

Billing is a planned product experience and must be designed together with the commercial entitlement architecture.

### Payment experience

For the Android Google Play version, digital purchases should use the planned Google Play Billing flow.

The UI should not make users enter card details directly into the quiz app. The app should present the product clearly and then hand the transaction to the supported Google Play purchase experience.

Conceptual journey:

```text
Home / Profile
      ↓
Premium / Store
      ↓
Choose product
      ↓
Google Play purchase flow
      ↓
Purchase confirmation
      ↓
Entitlement synchronization
      ↓
Premium feature/content unlocked
```

### Planned purchase surfaces

The redesign should account for:

- Premium landing/store screen.
- Monthly/yearly subscription cards where offered.
- One-time Remove Ads product.
- Premium content-pack cards.
- Product detail/benefit view where needed.
- Purchase confirmation state.
- Restore/synchronize purchases.
- Active Premium status.
- Expired/cancelled/pending/error states where relevant.

### UX principles

- Show the value before asking for payment.
- Make prices and billing periods obvious.
- Clearly distinguish subscriptions from one-time purchases.
- Never hide important terms.
- Do not use deceptive urgency.
- Do not create aggressive paywalls.
- Keep the free experience useful.
- Make restore/synchronization easy to find.
- Clearly communicate what the user's entitlement unlocks.
- Keep purchase errors understandable and actionable.
- Never make an unavailable payment method look available.
- Preserve the user's place in the app when returning from the purchase flow.

### Entitlement-aware UI

The interface should consume centralized entitlement state rather than independently deciding who is Premium.

Example:

```text
Entitlement: premium = active
        ↓
Premium category/mode cards become unlocked
        ↓
Locked features show clear benefits and upgrade action
```

This same design should work later for:

- `remove_ads`
- `bible_full`
- `current_affairs_pro`
- `advanced_stats`
- `endless_mode`
- future content packs

### Web/direct-payment future

A separate web purchase experience may be designed later for legitimately web-sold products. It should remain visually and commercially consistent with the app while respecting the platform rules governing links and purchases from the Android app.

### Current status

**Billing UI is planned, not implemented.** It belongs in the high-fidelity redesign/prototype phase before production billing code is added.

---

## 17. Loading, Error & Connectivity UX

Because online question delivery is required for quizzes while the Bible Library is intended to work offline, connectivity is a first-class global UX state.

Design states for:

- Loading questions
- Provider/API delay
- Network unavailable
- Empty result
- Temporary provider failure
- Retry
- Quiz unavailable
- Ad unavailable
- General unexpected error

Messages should be:

- Clear
- Short
- Non-technical
- Actionable where possible

Example direction:

**No internet connection**  
“Bible reading is available offline, but quizzes and online features require an internet connection.”

For quiz entry:

**Internet connection required**  
“Please connect to the internet to start a quiz.”

If the connection disappears:

**Connection lost**  
“Please reconnect to continue.”

**Quiz unavailable**  
“We couldn't load new questions right now.”

[ TRY AGAIN ]

Avoid exposing raw technical errors to normal users.

## 18. Navigation
### Future native Android navigation decision

The current React/Vite/Capacitor V1 UI is **not** the target implementation for the final app-wide navigation structure.

The future navigation architecture will be designed as an independent product/UX blueprint and implemented during the planned native Android migration. The current V1 remains stable and should not be reshaped into the future multi-section application merely to accommodate Bible.

The future navigation design should support:
- Clear top-level product destinations.
- Nested navigation inside major sections such as Bible.
- Predictable back/up behavior.
- Independent navigation history where appropriate.
- Scalable support for future account, progress, premium, settings and learning features.
- Bible-specific hierarchy without turning every Bible function into a top-level button.

Android's Navigation component supports navigation graphs, nested destinations, back stacks, and common patterns such as bottom navigation and navigation drawers; these patterns should inform the future native design without preselecting the final UI before wireframing. citeturn0search0

### Bible information architecture — future design

Bible should be treated as a major product section rather than a single quiz-category screen.

Conceptual hierarchy:
- Bible Home
  - Read Bible
    - Old Testament / New Testament
    - Book
    - Chapter
    - Bible Reader
      - Silent reading by default
      - User-selected Listen action
      - Play
      - Pause / Resume
      - Stop
      - Future speed/voice controls
  - Bible Quiz
    - Quick Quiz
    - Book Quiz
    - Chapter Quiz
    - Topic Quiz
  - Future study tools

**Voice reading must never auto-start when a chapter opens.** The reader opens in normal silent-reading mode. Audio begins only after the user explicitly chooses the listening action.

### V1 Bible placeholder UX

Until the native Bible experience is implemented, the current V1 Bible category should use a minimal, non-final placeholder such as:

**Bible — Coming Soon**
> A full Bible reading, study and quiz experience is being prepared for a future version of the app.

The placeholder should not expose a large collection of temporary buttons such as Read, Read Aloud, Study, Books, Chapters, etc. Those belong to the future navigation design.



Navigation should be simple and predictable, but the final architecture must support both quick quiz play and the deeper Bible reading experience.

We will decide during UX architecture whether the final product needs:

- Bottom navigation
- Top navigation
- Contextual back buttons
- Profile/settings access
- Home/category/game/result flow

Navigation must not compete with the quiz itself.

### Product information architecture direction

```text
DAILY QUIZ & CHALLENGE
│
├── Home
├── Quizzes
│   ├── General Knowledge
│   ├── Science
│   ├── Africa & Nigeria
│   ├── Current Affairs
│   └── Bible Quiz
├── Bible
│   ├── Read Bible
│   └── Bible Quiz
├── Results / Progress
└── Settings
```

This is an architecture direction, not a locked visual navigation implementation.

## 19. Component Design System

Reusable components to define:

- Primary button
- Secondary button
- Text/button variants
- Cards
- Category cards
- Daily Challenge card
- Answer options
- Progress bars
- Timer
- Score display
- Streak indicator
- Achievement badge
- Dialogs
- Snackbars/toasts
- Navigation
- Tabs if needed
- Loading indicators
- Error states
- Empty states
- Ad containers

Each component should have documented:

- Size
- Spacing
- Typography
- Radius
- Shadow/elevation
- Colour
- Interaction states
- Accessibility behaviour

## 20. Brand & Visual Identity

Eventually define:

- Logo treatment
- App icon
- Brand colour system
- Typography
- Icon style
- Illustration style
- Photography/image treatment if used
- Card style
- Shadow/elevation
- Corner radius
- Spacing scale
- Motion/animation language

The brand should communicate:

**Knowledge + challenge + energy + trust + modern technology.**

## 21. Micro-interactions & Motion

Animations should be:

- Fast
- Purposeful
- Subtle
- Satisfying
- Performance-conscious

Possible interactions:

- Button press feedback
- Answer selection
- Correct/incorrect feedback
- Progress movement
- Score reveal
- Streak celebration
- Achievement unlock
- Result completion
- Screen transitions

Avoid animation that delays the user or makes repeated quizzes tiring.

## 22. Accessibility

Accessibility is part of the design system, not an afterthought.

Requirements:

- Adequate text contrast
- Comfortable touch targets
- Readable font sizes
- Clear focus/selected states where relevant
- Do not rely on colour alone
- Clear error messages
- Avoid overly small interactive elements
- Avoid excessive motion
- Maintain usable layouts on smaller screens

## 23. UX Architecture Workflow

### Phase 1 — Product/UX Architecture
First map the complete product journey using the now-known category/provider capabilities. Do not finalize visual navigation before the Africa & Nigeria, Current Affairs and Bible architecture is sufficiently understood.

Define:

- Entry
- Home
- Category
- Configuration
- Ad transition
- Quiz
- Answer feedback
- Results
- Replay/next category
- Streaks
- Profile/statistics
- Settings
- Error/retry paths

### Phase 2 — Visual Identity
Lock and document:

- Royal Navy (`#0B2970`) as the brand anchor
- Blue, Electric Cyan and Warm Gold accents
- Light and dark surface/text tokens
- Typography and type scale
- App icon, logo mark and Android adaptive icon
- Native splash and first-launch brand intro
- Icon style, card style, spacing, shadows/elevation, corner radius and component rules
- Subtle premium background treatments and accessibility/performance limits

### Phase 3 — Wireframes
Design the complete native V2 screen inventory and journeys in Penpot, including account screens, before detailed implementation. Use the agreed design tokens while exploring hierarchy and layout.

Focus on:

- Hierarchy
- Flow
- Information placement
- Navigation
- Usability

### Phase 4 — High-Fidelity Design
Apply the final design system.

### Phase 5 — Interactive Prototype
Connect the screens into a tappable prototype.

Target workflow:

**Design → prototype → test on Oppo A56 → refine**

### Phase 6 — Implementation
Only after the design is approved:

**Approved Penpot design specification → Kotlin/Jetpack Compose native Android → Debug APK**

### Phase 7 — Regression Testing
After implementation, verify that the redesign did not break:

- Online questions
- Question history/anti-repetition behaviour
- Timer
- Scoring
- Streaks
- AdMob banner
- AdMob interstitial
- Rewarded ads
- App Open ads
- Loading/error states
- Release updates/install behaviour

## 24. Penpot Workflow (Active) 

Penpot is the active editable design/prototyping environment for Daily Quiz & Challenge V2. Figma may be used as a secondary reference or backup, but it is not the primary design workspace.

Target workflow:

**Product journeys → Penpot wireframes → high-fidelity light/dark designs → interactive prototype → phone testing/refinement → approved Kotlin/Jetpack Compose implementation → Debug APK testing**

Keep the workflow practical for the available mobile-first development setup. Repository documentation remains the durable record of approved design decisions, screen behavior and implementation status.

## 25. Design Rules

The product design philosophy is:

1. **Clean** — no unnecessary elements.
2. **Fast** — the user immediately understands what to do.
3. **Sharp** — strong typography and crisp components.
4. **Playful** — it is still a game.
5. **Premium** — it should not look like a beginner project.
6. **Accessible** — readable, high-contrast, comfortable and understandable.
7. **Consistent** — every screen belongs to the same application.
8. **Scalable** — new categories and features must fit without redesigning everything.

## 26. Implementation Safety Rule

**Do not begin major UI/UX code changes while the current functional foundation is still being stabilized.**

The current stable app is valuable.

The redesign should be developed from a known-good functional checkpoint so visual changes can be separated from functional debugging. Do not redesign only the current five-card screen and then retrofit Bible; design the final information architecture around the complete product.

The existing main `project.md` remains the source of truth for app-development status.

## 27. Current UI/UX Milestone

**Status: V2 product architecture approved; design-first specification underway.**

Completed:

- UI/UX research
- Behance inspiration review
- Initial visual direction
- Colour direction
- UX architecture direction
- Screen inventory
- Component inventory
- Ad UX principles
- Accessibility principles
- Penpot/prototype workflow
- Dedicated UI/UX project documentation

Not yet started:

- Final typography and detailed token values
- Final typography
- Wireframes
- High-fidelity native V2 screens
- Interactive Penpot prototype
- Native Compose UI implementation

## 28. Immediate UI/UX Next Steps — V2 DESIGN-FIRST MILESTONE

V1 is stabilized and remains the reference implementation on `main`. The major redesign belongs to native Android V2 on `v2-development`; do not retrofit this redesign into V1.

The approved sequence is:

1. Finalize app icon, logo/brand mark, Android adaptive icon and native splash direction.
2. Finalize the Royal Navy-led brand tokens, light/dark themes, typography, spacing, shapes, elevation and reusable components.
3. Define subtle premium screen-background treatments (soft glows, restrained tonal gradients, low-contrast abstract details and soft shadows) with clear limits against visual clutter and childish styling.
4. Finalize five-tab navigation and all primary/nested user journeys.
5. Wireframe and design the complete screen inventory in Penpot, including sign-in, registration and recovery screens.
6. Specify a short optional/skippable first-launch tutorial; it must not become a mandatory separate welcome screen.
7. Build an interactive Penpot prototype and review it on target Android screen sizes.
8. Approve the design package before implementing the native Kotlin/Compose foundation and screens.
9. Integrate email/password and Google authentication only after the native UI and account screens are ready.

Do not begin a V1 React/Vite redesign. Keep backend/provider work synchronized, but do not let it displace the approved design-first sequence.

## 29. Long-Term UI/UX Goal

The finished Daily Quiz & Challenge interface should feel like a product that can grow for years.

It should support future expansion such as:

- More categories
- Bible book/chapter selection
- Science subcategories
- Africa & Nigeria content
- Current Affairs
- User accounts
- Statistics
- Achievements
- Streak systems
- Explanations/learning
- Rewards
- More game modes
- Future backend-driven personalization

The design system must therefore be built for **long-term scalability**, not just the next APK.

---

## 30. Current synchronized product direction

**Bible:** World English Bible (WEB), Catholic edition / Catholic book order (WEBC). Official source/licensing research is established; the source is public domain and the official distribution includes structured and read-aloud source formats. The full Bible reader, quiz modes and explicit user-controlled voice reading belong to the future native Android experience, not the current V1 UI. citeturn0search1turn0search2

**Connectivity:** The redesigned app needs a centralized connectivity state. Online-required features should clearly explain the requirement; offline Bible reading should remain usable.

**Provider sequence:** General Knowledge and Science are complete; Africa & Nigeria and Current Affairs are next research targets; Bible architecture/provider research continues in parallel.

**Design sequencing:** Define the remaining product architecture first, then redesign the UI/UX once around the complete experience. Keep this file synchronized with `project.md`.

## Design North Star

> **Daily Quiz & Challenge should feel like a polished modern quiz product: clean white surfaces, sharp blue actions, dark navy hierarchy, a carefully controlled reward accent, excellent spacing, satisfying interactions, fresh content, and a gameplay experience people want to return to.**

**Design first. Code second. Test everything. Protect the stable foundation.**

## Phase 3G UX alignment — 24 September 2026

The Current Affairs Quiz Assembler establishes the functional rules that the future UI/UX must represent clearly: a standard 10-question session, balanced difficulty, varied content, no same-family repeats, recent-question avoidance, and a clear insufficient-content/error state when a safe full quiz cannot be assembled.

These are backend/content-selection constraints, not visual decisions. The major UI redesign remains deferred until the functional V1 foundation is stable.

## Current Affairs Question Bank checkpoint — 24 September 2026

The Current Affairs architecture now includes a dedicated Question Bank as a planned content layer, but **the persistent Question Bank storage/management implementation is not yet complete**.

The future UI should consume curated validated question records, not generated drafts or user history. Question records will retain stable identity, family/concept relationships, difficulty, explanations, provenance-aware metadata and future FREE/PREMIUM/SPECIAL_PACK access metadata.

Worker integration is intentionally postponed until the Question Bank layer is implemented and audited. No UI redesign is required for this backend checkpoint.

## Current Affairs Phase 3H UI/UX architecture checkpoint — 24 September 2026

Phase 3H Question Bank Storage & Management is complete through 3H-E. The UI/UX layer remains intentionally decoupled from Question Bank storage and generation.

The future Current Affairs experience can therefore consume a clean serving layer without exposing internal fact IDs, source records, generator metadata or Question Bank lifecycle details to users.

UI/UX implications preserved for later implementation:
- Quiz screens receive playable question records only.
- Access-tier metadata can later support free/premium/special-pack presentation without hard-coding billing into question components.
- Explanations and future source attribution can be surfaced after answers where appropriate.
- Dynamic/current facts may require clear date/context presentation in the final question or explanation.
- Loading, empty-bank, insufficient-question, stale-content and network/error states should be designed before Worker integration is exposed to users.
- The existing UI redesign remains deferred until the functional Current Affairs pipeline is stable.

**Phase 3H status: complete. Next functional stage: Worker integration, then debug/end-to-end testing.**


## Current Affairs Phase 3I-A / 3I-B UX alignment checkpoint — 24 September 2026

The Current Affairs backend now has a real Worker serving boundary and a bounded Question Bank seed.

The future UI should treat the Worker as the source of a complete quiz session rather than requesting individual difficulty groups independently. This preserves global family, concept, recent-history and difficulty rules.

The UI must handle these backend states cleanly:
- complete 10-question response;
- no eligible questions after recent-history filtering;
- Question Bank not populated/temporarily unavailable;
- invalid or failed Worker response;
- network failure;
- future premium/special-pack access restrictions.

No internal Question Bank IDs, family IDs, source IDs, generator versions or lifecycle metadata should be shown during normal gameplay.

The major UI redesign remains deferred. The next UI-relevant work is end-to-end debug testing of the existing app against the real Current Affairs Worker path.


## Current Affairs implementation-complete checkpoint — 24 September 2026

The V1 **Current Affairs category implementation is now complete for the current stabilization milestone**.

Completed implementation chain:
- Verified fact-first Current Affairs knowledge base and source/provenance model.
- Phase 3A–3H question model, blueprints, generation, distractors, quality validation, duplicate/family detection, quiz assembly, and Question Bank management.
- Phase 3I Worker integration and version-controlled serving Question Bank.
- Phase 3J runtime generation/coverage path for recovery when recent-history filtering exhausts the seeded bank.
- Current Affairs remains isolated from NewsData; NewsData is reserved for the future News Quiz / Current Events product.
- Recent-history exhaustion testing successfully returned a complete 10-question quiz without re-serving the 20 supplied recent-history IDs.
- Runtime exhaustion recovery is therefore considered functionally established for this V1 milestone.
- The preferred 4 Easy / 4 Medium / 2 Hard distribution may relax when the available eligible pool is constrained by recent-history/exhaustion rules; anti-repeat protections remain in force.
- Known Question Bank explanation/content cleanup items observed during testing are recorded as a later cleanup task and are **not** being changed in this checkpoint.

### Next controlled release steps

1. Merge the completed Current Affairs implementation pull request into `main`.
2. Confirm the merged `main` state and production Worker deployment.
3. Build the Android **Debug APK** from the merged state.
4. Install/update the Debug APK on the test phone.
5. Run end-to-end V1 category testing, with Current Affairs included, while protecting the already-confirmed Science, General Knowledge, Africa & Nigeria and AdMob behavior.
6. Record any findings as stabilization fixes only; do not begin the major native Android/UI redesign during this test cycle.

**Current milestone:** Current Affairs implementation complete → PR merge → Debug APK validation → V1 stabilization.


## V1 stabilization complete — 24 September 2026

The V1 functional reference app is now officially stabilized. The current React/Vite/Capacitor interface should be treated as a **frozen reference baseline** rather than the target for the major redesign.

The next product-design phase moves to the separate native Android V2 architecture. Future app-wide navigation, the full Bible experience, major UI/UX redesign, design-system implementation, and native Android interaction patterns should be designed for V2 rather than retrofitted into V1.

The V1 AdMob behavior must be preserved as a functional reference during the V2 migration: Debug uses test ads, release uses production AdMob IDs, and ad containers/presentation must remain regression-tested in the native implementation.

**V1 UI/UX status: STABILIZED REFERENCE.**

**Next milestone: Native Android V2 product architecture and UX design.**


## 29. Ricven alignment and design workflow

### Penpot + Claude

- **Penpot is the active design workspace** for wireframes, screen layouts, reusable components, prototypes and design-system exploration.
- **Claude is an AI-assisted collaborator** for design exploration, flow review, specification refinement and implementation guidance.
- Penpot remains the editable visual source for approved designs. Keep approved requirements, design decisions, component behaviour and implementation status recorded in this repository.
- Claude-generated suggestions are proposals until reviewed and accepted; never silently convert them into confirmed product decisions.
- Figma may remain a secondary reference, but Penpot is the active primary design tool.
- Review every proposed design against the current product architecture and exact shared palette before implementation.

### Premium redesign — five experience pillars

1. **Upgraded Home Dashboard:** clear Daily Challenge entry, streak/progress summary, category discovery, useful quick actions and meaningful return-user context.
2. **Immersive Quiz Experience:** focused question canvas, clear progress/timer hierarchy, comfortable answer targets, accessible feedback, subtle transitions and robust loading/network/error states.
3. **Achievements & Competition:** points, streak milestones, badges, trophies, personal bests, challenges and leaderboards. Early competition should be non-cash.
4. **Better Category Experiences:** dedicated category landing pages, useful subtopic discovery, category context and clear availability/access states within one shared component system.
5. **Learning & Performance Centre:** quiz history, accuracy/topic trends, difficulty/category breakdowns, strengths, improvement areas and actionable practice recommendations. Distinguish local V1 metrics from future account-synced analytics.

These five pillars are design targets, not a statement that all features are implemented in V1.

### Design-to-implementation acceptance

Before implementation, each major flow should include Penpot frames for primary, empty, loading, error and relevant offline states; a component inventory and interaction specification; light/dark token mapping; compact and large-screen checks; accessibility checks; and a traceable link to the corresponding repository requirement.


## V2 native navigation and account decisions — 3 November 2026

**Status: approved information architecture; detailed wireframes and visual specifications remain to be designed.**

The native Android V2 application will use five persistent primary navigation destinations:

1. **Home** — dashboard, Daily Challenge entry, streak/progress overview and quick actions.
2. **Explore** — categories, subjects, topics, content discovery and quiz configuration.
3. **Challenges** — daily and special challenges, participation and challenge history.
4. **Rankings** — leaderboards, rankings and competitive progress.
5. **Profile** — account/profile, personal quiz history, statistics, achievements and settings entry points.

Use a persistent bottom navigation bar for these five top-level destinations. Do not add quiz, results, category detail, authentication, settings or achievement detail as additional primary tabs. Those are nested destinations within the relevant section or app-level flows.

### Navigation behavior requirements

- Each top-level destination should retain appropriate navigation state/back stack when users switch tabs.
- Define clear Back and Up behavior, including from nested flows and authentication.
- Support deep links into eligible destinations without bypassing authentication/authorization.
- Preserve navigation and quiz state through configuration changes and process recreation where appropriate.
- Show clear selected/unselected states and accessible labels for every destination.
- Provide loading, empty, offline, error and retry states.
- Authentication, registration, sign-in and recovery are flows, not bottom navigation destinations.
- Public browsing may be available without an account; personal history, synchronized progress and protected actions require sign-in.

### Account entry and authentication UX

V2 will support both **email/password** and **Google sign-in**. Design the account journey to cover:
- Sign up and sign in.
- Google identity-provider handoff and return.
- Email verification where required.
- Password recovery/reset.
- Session expiry, re-authentication and sign-out.
- Profile editing and preferences.
- Account deletion and clear privacy/data-retention information.
- Understandable offline, provider failure and retry states.

Do not imply that a user is authenticated merely because a Google account chooser completed. The trusted backend must verify the identity and issue the application session.

### Screen inventory to wireframe

In addition to the five tab roots, plan nested screens for category/topic detail, quiz setup, active quiz, answer feedback, results, quiz history, challenge detail/participation, leaderboard detail, achievement detail, profile edit, settings, sign-in, registration, verification and recovery.

### Visual design constraints

Continue using the approved shared Ricven/RichInsights palette, with **Royal Navy** (`#0B2970`) as the brand anchor. The five-tab structure is approved. Final icon, typography, splash, adaptive icon, light/dark theme, component dimensions, subtle background treatments, responsive behavior and screen compositions must be designed and reviewed in Penpot before native implementation.

## V2 brand and native UI implementation plan — 4 November 2026

**Status:** Approved direction and implementation sequence; no claim that assets, Penpot screens or native Compose code are already complete.

### Brand identity and Android launch assets
- Create a distinctive app icon and matching logo/brand mark that work at small sizes and across light/dark contexts.
- Produce Android adaptive icon foreground/background assets and verify launcher masking at common shapes.
- Specify the native Android system splash using the approved brand mark and Royal Navy-led background.
- Define a short first-launch-only brand intro if needed, with mark, app name and tagline; it must be skippable and must not delay Home or depend on network/auth initialization.

### Design system
- Royal Navy `#0B2970` is the correct name for the brand anchor. Do not call it Deep Navy.
- Retain Blue `#1769E0`, Electric Cyan `#19B5FE` and Warm Gold `#F4B942` with the restrained usage rules above.
- Define semantic colors, light/dark surfaces, typography, spacing, shape, border, elevation and component tokens once, then reuse them throughout Compose.
- Define a small, consistent elevation scale: soft shadows for separation, not heavy floating-card effects.

### Premium background visual language
- Use subtle ambient glow or soft radial lighting only where it supports hierarchy (for example, a Home hero, challenge highlight or selected header region).
- Use low-contrast abstract geometry, fine tonal texture or barely visible patterns sparingly; avoid repeating decoration on every card.
- Light theme should remain bright, crisp and breathable; dark theme may use soft Royal Navy/Blue tonal depth with restrained Cyan highlights.
- Prefer layered tonal surfaces and carefully controlled edge/shadow treatment over loud multi-stop gradients, neon glow, excessive blur, glassmorphism or busy illustrations.
- Keep content, controls and text dominant. Background decoration must never reduce contrast, legibility, touch clarity or performance.

### Navigation, onboarding and account screens
- Implement five persistent destinations: Home, Explore, Challenges, Rankings and Profile, with nested destinations and predictable Back/Up behavior.
- Design authentication as separate flows, not bottom-navigation tabs. Sign-in, registration, Google handoff, verification and recovery screens are included in the early Penpot design package.
- Add a short optional, skippable first-launch tutorial only if it improves discovery. It is an overlay/sequence that can be skipped and revisited from Help/Settings if appropriate, not a mandatory standalone welcome screen.
- Do not gate Home behind the tutorial, login, remote configuration, ads or network availability. Guest browsing remains subject to the product decision on which public features can be used without an account.

### Design and implementation acceptance
- Penpot is the active design source; Figma is secondary reference only.
- Each major flow must include primary, loading, empty, error and relevant offline states, plus light/dark variants where applicable.
- Approve icon/splash assets, tokens, screen layouts, interactions, accessibility checks and responsive behavior before corresponding Compose implementation.
- Login/registration UI is designed early; real email/password and Google sign-in integration comes later, after the native UI foundation and screens are in place.
- Keep V1 source, package/signing identity, production Worker routes and release behavior untouched.

