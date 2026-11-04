# Daily Quiz & Challenge — UI/UX Authority

**Status:** V2 design direction approved; detailed design work is the next project milestone.
**Design workspace:** Penpot (active); Figma is reference/backup only.

This document is the sole authority for V2 brand identity, visual tokens, typography, information architecture, navigation UX, screens, components, interaction states, accessibility and design/prototype decisions. The production sequence is owned by project.md. Android implementation is owned by v2-native-architecture.md.

Historical V1 notes are retained only where they explain a design constraint; they do not override the approved V2 direction.

## 1. Design Project Status

V2 is a native Kotlin/Jetpack Compose product. The approved next design work is Brand & Foundations, followed by user journeys, Penpot wireframes, high-fidelity light/dark designs and a clickable prototype. Account screens are designed with the UI; live authentication integration follows the native UI foundation and screen implementation.

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

| Token | Hex / status |
|---|---|
| Success | `#22A06B` |
| Success Text (light) | `#167A4C` |
| Error | `#C93B3B` |
| Error Text (dark) | `#FFB4AB` |
| Warning | To be defined and contrast-tested |
| Disabled | To be defined for light and dark |
| Success Text (dark) | To be defined and contrast-tested |

### Usage and accessibility rules

- Keep the brand primitives consistent in light and dark themes; adapt surfaces and text using the tokens above.
- Gold is an achievement accent only; do not use it for routine navigation, generic actions or ordinary body text.
- Cyan and Blue carry interaction, selection and progress emphasis. Avoid making every element bright.
- Until measured contrast proves otherwise, use Cyan and Gold as non-text accents on light surfaces; do not use either as small text on white.
- Define warning, disabled and dark-mode success-text tokens before high-fidelity design is signed off.
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

No typeface has been approved yet. Evaluate two suitable candidates, then choose one and define the full eight-level hierarchy as reusable tokens. Do not imply that a typeface is already selected.

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

## 18. Navigation and information architecture

The approved V2 primary navigation has exactly five destinations:

1. **Home** — dashboard, progress overview, quick actions and a Daily Challenge entry card.
2. **Explore** — category, subject and topic discovery, including the proposed home for Bible unless the UX review selects another nested entry.
3. **Challenges** — daily and special challenges, participation and challenge history.
4. **Rankings** — leaderboard and competitive progress.
5. **Profile** — account, personal history, statistics, achievements and settings entry points.

Quiz, answer feedback, results, authentication, settings, Premium/Store, category/topic details and achievement details are nested flows, not primary tabs. Premium/Store must not become a sixth destination.

### Navigation behavior

- Use a persistent five-destination app shell outside focused task flows.
- Preserve appropriate tab state/back stacks when switching destinations.
- Define Back/Up, deep links, process restoration and authentication gates.
- Keep active quiz play focused. **Proposal for approval:** hide the bottom navigation while a quiz is active.
- Every major flow needs primary, loading, empty, error and relevant offline states in light and dark themes.
- Public browsing and guest quiz permissions remain subject to the guest-access decision.
- Answer feedback must communicate correctness with text, icons and shape/state treatment; colour is never the only signal.

### Open UX decisions — do not assume

1. Can guests play quizzes, and which actions trigger sign-in?
2. Is Bible nested inside Explore, and what is its entry and navigation hierarchy?
3. How does the Home Daily Challenge card relate to the Challenges destination, schedule and participation history?
4. Does News Quiz appear in V2, and if so, where is its entry point?
5. Confirm whether the bottom bar is hidden during an active quiz.
6. Are Rankings global, country-based, friends-based or a combination? What public profile fields are shown?
7. Confirm the exact Premium/Store placement; Profile and contextual entry points remain proposals until approved.

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

## 23. Design workflow and handoff

Follow the single project sequence in project.md. Within the design work, use this handoff:

**Product journeys → Penpot wireframes → high-fidelity light/dark designs → clickable prototype → device review/refinement → approved Compose implementation.**

Penpot is the editable design source. Record approved frames, interactions, tokens and acceptance criteria here. Do not create a second numbered production roadmap in this document.

## 24. Penpot Workflow

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

The project roadmap and overall production status are governed by project.md.

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

- Final typeface and detailed token values
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

## Historical V1 Current Affairs UX constraints

The V1 Current Affairs work established durable UX requirements that remain relevant to V2:

- Serve complete quiz sessions through the backend rather than requesting separate difficulty batches from the client.
- Present clear unavailable, insufficient-content, network/provider failure and retry states.
- Keep internal fact IDs, question-family IDs, source IDs, generator metadata and lifecycle details out of normal gameplay.
- Surface explanations and source attribution after answers where useful.
- Keep Current Affairs fact-first and separate from the News Quiz live-news experience.

The V1 implementation and its historical engineering checkpoints are documented in backend-architecture.md. They do not define a separate V2 design roadmap.

## V2 navigation and account decisions

The five primary destinations, account journey and nested screen inventory above govern the V2 design. Public browsing may be available without an account; personal history, synchronized progress and protected actions require sign-in, subject to the guest-access decision in project.md.

V2 supports email/password and Google sign-in. Design registration, sign-in, verification, recovery and account-management screens early; real authentication integration follows the native UI foundation.

## V2 brand and native UI implementation direction

- Reuse the shared Royal Navy, Blue, Electric Cyan and Warm Gold brand primitives.
- Complete light/dark semantic tokens, typography, spacing, shapes, elevation and reusable components before hi-fi sign-off.
- Keep premium background treatments restrained, readable and performant.
- Design the optional/skippable first-launch tutorial without making it a mandatory welcome barrier.
- Keep Home available during auth, ads, remote configuration and network failures.
- Preserve V1 source, package/signing identity, production Worker routes and release behavior.

**Design acceptance:** approved Penpot source, complete primary/nested flows, light/dark states, accessibility review, and prototype review on target Android screen sizes.