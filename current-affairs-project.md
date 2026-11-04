# Current Affairs Architecture & Source Registry

**Status:** V1 Current Affairs pipeline implemented and production-verified at the documented stabilization checkpoint. V2 content architecture extends it with controlled AI-assisted generation.

## 1. Purpose

The V1 **Current Affairs** category is a structured knowledge system, not a live-news headline quiz.

It must cover current civic/general knowledge across Nigeria, Africa and the wider world, including government, institutions, geography, economy, international organizations, sports, science and technology.

A separate future product, **News Quiz / Current Events**, may use NewsData.io or another news provider for actual recent-news questions. That system must remain isolated from the V1 Current Affairs knowledge system.

## 2. Isolation boundary: Current Affairs vs News Quiz

### V1 Current Affairs

```text
Trusted / official factual sources
        ↓
Current Affairs Knowledge Base
        ↓
Fact verification + validity dates
        ↓
Current Affairs Question Generator
        ↓
Question Quality Validator
        ↓
Existing Question Engine
        ↓
10-question quiz
```

### Future News Quiz / Current Events

```text
NewsData.io (or future news provider)
        ↓
News Article / Event ingestion
        ↓
News-specific filtering
        ↓
News Quiz Question Generator
        ↓
News Quiz validation
        ↓
Future News Quiz experience
```

### Hard isolation rules

1. Existing NewsData code must **not** be used as the provider for the V1 Current Affairs knowledge system.
2. NewsData helper functions should eventually live behind an explicitly news-specific provider/module boundary, using names such as `news_quiz`, `current_events`, `newsProvider`, or equivalent.
3. V1 Current Affairs must have its own provider/source registry, fact model, generator and validation path.
4. A change to NewsData parsing, article filtering or news-question templates must not change Current Affairs fact generation.
5. A change to Current Affairs fact schemas or generators must not change NewsData behavior.
6. Shared code is allowed only for genuinely generic utilities such as JSON/CORS helpers, HTTP timeout handling, safe shuffling and common question validation. Provider-specific logic must not be shared.
7. The category router must explicitly distinguish `current_affairs` → Current Affairs knowledge engine and future `news_quiz` / `current_events` → NewsData/news-provider engine.
8. NewsData credentials must remain scoped to the future news provider and must never become a dependency of the Current Affairs knowledge path.

### Planned Worker structure

```text
worker/
├── index.js                    # routing only / compatibility layer
├── current-affairs/
│   ├── sources.js              # source registry
│   ├── facts.js                # fact retrieval/normalization
│   ├── generator.js            # question generation
│   ├── validator.js            # quality checks
│   └── index.js                # Current Affairs service
└── news-quiz/
    ├── provider.js             # NewsData/future news provider
    ├── generator.js            # news-specific questions
    ├── validator.js
    └── index.js                # future News Quiz service
```

This is the target organization. Do not perform the refactor until the current V1 category work is ready for it; the first implementation step should be additive and carefully tested.

## 3. Current Affairs domains

### 3.1 Nigeria
- Federal Government
- National Assembly
- Judiciary
- current political leadership and public officeholders
- governors and state structure
- ministries/agencies
- political parties and electoral facts
- 36 states + FCT
- geopolitical zones
- Nigerian geography
- cities, rivers, lakes, mountains and borders
- history and important dates
- Constitution and democratic institutions
- economy and national institutions
- national symbols
- important current appointments

### 3.2 Africa
- countries, capitals and regions
- geography
- rivers, lakes and mountains
- African Union
- ECOWAS and other regional institutions
- African leaders and institutions
- history
- economies and currencies
- important current institutional/development facts

### 3.3 World
- countries and capitals
- continents, oceans and geography
- major public leaders where appropriate
- international institutions
- economies and currencies
- international organizations
- current international facts

### 3.4 International Organizations
Examples include:
- United Nations
- African Union
- ECOWAS
- World Bank
- IMF
- WHO
- UNESCO
- UNICEF
- WTO
- NATO
- Commonwealth
- OPEC

Question targets include leadership, headquarters, purpose, membership, founding, functions and other verified institutional facts.

### 3.5 Economy & Business
- currencies
- central banks
- inflation and economic concepts
- major economic institutions
- GDP/general economic facts
- Nigerian economic institutions
- international financial organizations
- commodities and trade organizations

Dynamic numeric indicators must carry a year/dataset vintage.

### 3.6 Geography
- countries
- capitals
- continents
- oceans and seas
- rivers and lakes
- mountains and deserts
- borders
- regions
- population/geographic facts
- Nigerian, African and world geography

### 3.7 Sports
- major competitions
- countries and teams
- championships
- Olympics
- football
- athletics
- basketball
- sports organizations
- important current records/holders where the record is explicitly sourced and dated

### 3.8 Science & Technology
- inventions
- space
- technology
- AI
- scientific institutions
- discoveries
- current technological developments

## 4. Fact-first architecture

Current Affairs stores **facts**, not only prewritten questions.

Example:

```json
{
  "id": "ng-federal-president",
  "domain": "nigeria",
  "topic": "government",
  "entity": "President of Nigeria",
  "attribute": "currentHolder",
  "value": "...",
  "validFrom": "...",
  "validTo": null,
  "lastVerified": "...",
  "source": "official-source-name",
  "sourceUrl": "https://...",
  "status": "active"
}
```

Recommended fields:

| Field | Purpose |
|---|---|
| `id` | Stable fact identifier |
| `domain` | Nigeria/Africa/World/etc. |
| `topic` | Government/geography/economy/etc. |
| `entity` | Thing/person/institution being described |
| `attribute` | Property being tested |
| `value` | Verified value |
| `validFrom` | Start of validity |
| `validTo` | End when known |
| `lastVerified` | Last verification timestamp/date |
| `source` | Human-readable source name |
| `sourceUrl` | Direct source |
| `status` | active/superseded/review/invalid |
| `notes` | Optional context/qualification |

For changing facts, update or supersede the **fact record**, rather than manually editing hundreds of questions.

## 5. Source registry — Phase 1

### Tier 1: official/primary sources

| Domain | Source | Intended use | Status |
|---|---|---|---|
| Nigeria | Federal Government / official government portals | federal structure, public offices, government facts | approved candidate |
| Nigeria | INEC | states, FCT, LGAs, electoral/geographic facts | approved |
| Nigeria | National Assembly official sources | Senate/House leadership and institutional facts | research next |
| Nigeria | Presidency / State House | Presidency, appointments and executive facts | research next |
| Nigeria | CBN | monetary/economic facts and current institutional leadership | research next |
| Africa | African Union | member states, regions, institutions | approved |
| Africa | ECOWAS | regional membership and institutional facts | approved with freshness checks |
| World | United Nations | member states, international institutions | approved |
| Economy | World Bank Open Data | country/economic/development indicators | approved with year/vintage |
| Economy | IMF WEO/data | macroeconomic indicators and projections | approved with dataset vintage |
| International | WHO / UNESCO / UNICEF / WTO / OPEC / NATO / Commonwealth official sources | organization-specific facts | research/verify before ingestion |

### Verified Phase 1 sources

**INEC:** its official State Offices directory identifies 36 states + FCT (37 offices), 774 LGA offices, capitals and geopolitical-zone groupings. citeturn0search0

**African Union:** the official AU member-state page lists 55 member states. Membership/status questions must account for the AU's own suspension notes and current page wording rather than copying stale secondary summaries. citeturn0search1

**ECOWAS:** the official ECOWAS member-state page provides its membership information; because regional membership can change, production facts must be verified against the current official page during updates. citeturn0search2

## 6. Source priority

When multiple sources cover the same fact:

1. Constitutional/statutory or official government source.
2. Official institution/organization source.
3. High-quality primary dataset from an intergovernmental institution.
4. Reputable secondary reference only when a primary source is unavailable.
5. General web pages only as discovery aids, not as the final authority for time-sensitive facts.

For political officeholders and institutional leadership, use the institution's official source wherever possible. Questions must remain factual and non-persuasive.

## 7. Verification and update rules

### Time-sensitive facts
Examples:
- officeholders
- political party leadership
- appointments
- institutional leadership
- current records
- membership/status changes
- economic indicators
- current technological developments

Rules:
- store `lastVerified`
- store `validFrom` and `validTo` where applicable
- store source and source URL
- reverify on a defined schedule
- immediately supersede a fact when an authoritative change is confirmed

### Stable facts
Examples:
- capitals
- continent membership
- founding dates
- physical geography
- constitutional structures that have not changed

These still require source attribution and periodic review, but they do not need the same update frequency as officeholders.

### Dynamic numeric facts
Every population, GDP, inflation, ranking, count or projection must include:
- reference year/period
- dataset/provider
- dataset vintage where applicable

Never present a dated statistic as an undated permanent fact.

## 8. Question-generation system

The generator should derive multiple question forms from a verified fact instead of repeatedly asking one template.

Supported forms:

1. Direct recall
2. Reverse question
3. Identification
4. Classification
5. Relationship
6. Number/count
7. Chronology/date
8. Matching
9. Scenario/application
10. Odd-one-out
11. Institution/function
12. Multi-fact reasoning/comparison

Example fact:

```json
{
  "country": "Nigeria",
  "capital": "Abuja",
  "continent": "Africa",
  "region": "West Africa",
  "currency": "Naira"
}
```

Possible generated targets:
- capital of Nigeria
- country whose capital is Abuja
- region containing Nigeria
- country using the naira
- relationship between Nigeria, Africa and West Africa

The generator must not expose the source fact as a clue that gives away the answer.

## 9. Difficulty model

**Easy**
- direct recognition
- common institutions
- simple geography
- common factual relationships

**Medium**
- reverse questions
- related entities
- classification
- plausible/similar distractors
- cross-attribute relationships

**Hard**
- multi-fact reasoning
- comparisons
- chronology
- institutional distinctions
- carefully constructed near-miss distractors

Difficulty must reflect reasoning demand, not obscure trivia alone.

## 10. Quality validator

Before a question enters the production question pool:

- source fact is verified
- answer is uniquely correct
- all distractors are plausible but wrong
- wording is unambiguous
- political questions are neutral and factual
- dates/years are explicit where needed
- dynamic facts include their time context
- exactly four options for standard MCQ
- explanation/source metadata is retained where applicable
- duplicate question IDs are rejected
- semantic duplicates are rejected where practical
- stale/superseded facts cannot generate active questions

## 11. Question identity and anti-repetition

Question IDs must be stable and unique.

A generated question should retain:
- fact ID(s)
- template/type
- source
- generation version

Conceptual ID: `ca:<fact-id>:<template>:<generation-version>`

This allows the existing `recent_history` system to prevent repeats while still allowing a changed fact to produce a new valid question version.

## 12. Content pipeline and readiness

The Current Affairs content lifecycle is:

**Source discovery → verified fact ingestion → normalized fact/source registry → question blueprint and generation → distractor construction → quality/freshness validation → duplicate/family checks → approved Question Bank → quiz assembly and serving.**

This is a content lifecycle, not a competing project roadmap. Project-wide sequencing is owned by project.md; generic Cloudflare AI service implementation is owned by backend-architecture.md.

## 13. Scope boundaries

- Keep Current Affairs fact-first and separate from News Quiz / Current Events.
- Do not replace verified facts with model-generated claims.
- Do not use NewsData or other live-news article feeds as the Current Affairs fact source.
- Keep user recent-question history separate from the Question Bank.
- Do not expose source IDs, fact IDs, question-family IDs or generation metadata in ordinary gameplay.
- Preserve V1 routes and category behavior while V2 is developed.
- Keep billing and entitlement enforcement outside the content generator.

## 14. Content acceptance criteria

A Current Affairs question is eligible for serving only when:

- Its underlying fact has an authoritative source and valid provenance.
- Time-sensitive values have verification and validity dates; numeric statistics carry a reference period and dataset vintage.
- The correct answer is uniquely defensible and distractors are plausible but incorrect.
- Wording is neutral, clear and unambiguous.
- Difficulty reflects reasoning demand, not obscure trivia alone.
- Exact, semantic and question-family duplicates are rejected or cooled down.
- Superseded/stale facts and their dependent questions are retired or revalidated.
- The question retains source/fact identity, generation/version metadata and validation outcome.
- The assembler respects category/topic balance, difficulty, freshness and user history.

## 15. V2 AI-assisted generation

AI is a required V2 generation capability and a major way to expand question variety and coverage. It supplements the verified fact-first system; it does not replace the source registry or become the authority for factual truth.

**V2 flow:**

```text
Verified Current Affairs facts + trusted source context
                    ↓
        AI-assisted question drafting
                    ↓
       Deterministic and AI-assisted checks
                    ↓
  Answer/distractor/explanation/freshness validation
                    ↓
      Duplicate and question-family checks
                    ↓
         Review where risk requires it
                    ↓
          Approved Question Bank
                    ↓
              Quiz serving
```

- Ground every factual draft in one or more approved fact records and preserve their source references.
- AI may propose alternative question forms, explanations, difficulty variants and distractors; it must not invent unsupported facts, dates, officeholders or statistics.
- Validate the output against the underlying fact and current source. A model's confidence or a second model's agreement is not sufficient evidence by itself.
- Quarantine low-confidence, ambiguous, disputed, political, rapidly changing or otherwise sensitive material for human review.
- Keep a fact's validity window attached to every dependent question. When a fact changes, retire or revalidate all affected variants.
- Preserve concept/family identity across paraphrases so AI-generated variations do not appear as new concepts or repeat in the same quiz.
- Batch-generate into a staging area. Runtime AI generation is a controlled fallback only; never bypass the same gates to fill a quiz.
- Keep model, prompt-template, source and validation metadata for auditability. Do not store secrets or private model reasoning.

News Quiz remains separate: AI-generated questions must be grounded in the specific retrieved article, preserve article URL/source/time context, and expire according to the article's freshness policy.

## Phase 3 — Question-generation architecture and plan

Phase 3 begins only after the Phase 2 fact dataset and the NewsData isolation are complete.

### Core architecture

```text
VERIFIED FACTS
      ↓
FACT RELATIONSHIP MAP
      ↓
QUESTION BLUEPRINTS
      ↓
Question Generator + Distractor Generator
      ↓
QUALITY VALIDATOR
      ↓
Validated Question Pool
      ↓
QUIZ ASSEMBLER
      ↓
Existing Quiz Engine
```

### Core identity model

The system will treat questions as relationships, not merely strings:

```text
FACT
  ↓
CONCEPT
  ↓
QUESTION FAMILY
  ↓
QUESTION VARIANTS
```

For example, the fact that Nigeria's capital is Abuja can produce direct and reverse variants, but both belong to the same `questionFamilyId`. A single 10-question quiz must never contain two variants from the same family. User history should also support a longer family/relationship cooldown where practical.

**Important rule:** different wording does not automatically mean a different question.

### Planned question types

The generator may support:

- Direct recall
- Reverse
- Identification
- Classification
- Relationship
- Institution/function
- Comparison
- Number/count
- Chronology/date
- Matching
- Scenario/application
- Odd-one-out
- Multi-fact reasoning

Question-type variety is useful, but **quality takes priority over forced variety**.

### Difficulty model

- **Easy:** direct recognition, common institutions, simple geography and straightforward facts.
- **Medium:** relationships, reverse questions, classification and distinctions between similar facts.
- **Hard:** multi-fact reasoning, comparisons, chronology, institutional distinctions and carefully constructed distractors.

Difficulty must describe the actual reasoning required rather than being assigned merely to make a 3/4/3 quiz mix.

### Question provenance model

Generated questions should retain enough metadata to trace them back to verified facts:

```json
{
  "questionId": "...",
  "factIds": ["ng-federal-structure"],
  "conceptId": "nigeria-federal-structure",
  "questionFamilyId": "nigeria-federal-units",
  "variantType": "direct",
  "difficulty": "easy",
  "question": "How many states does Nigeria have?",
  "options": ["36", "30", "37", "40"],
  "correctAnswer": "36",
  "status": "active",
  "lastValidated": "2026-09-23"
}
```

Future access metadata may also support `FREE`, `PREMIUM` and `SPECIAL_PACK`, but entitlement enforcement remains outside Phase 3.

### Distractor generation

Distractors must be:

- Plausible
- Semantically related
- Factually incorrect
- Unambiguous
- Appropriate to the domain
- Appropriate to the difficulty

The system must avoid obviously silly distractors and distractors that could also be technically correct.

### Question lifecycle

```text
generated
   ↓
validated
   ↓
active
   ↓
used / revalidated
   ↓
superseded or retired
```

If a source fact changes, dependent questions must be identifiable and eligible for revalidation or supersession.

### Quality validation

Every generated question should pass checks for:

- Factual correctness
- Answer uniqueness
- Distractor validity
- Language clarity
- Difficulty suitability
- Exact/semantic duplication
- Question-family duplication
- Fact freshness
- Source validity
- Option integrity

Rejected-question reasons should be explicit, such as:

- duplicate
- semantic duplicate
- family duplicate
- ambiguous
- bad distractor
- unsupported fact
- stale fact
- grammar problem
- poor difficulty
- insufficient information

### Quiz-level assembly rules

The future assembler should enforce:

- No duplicate question IDs
- No duplicate question families
- No excessive concentration on one topic
- Reasonable difficulty distribution
- Reasonable question-type diversity
- No stale facts
- User recent-history avoidance

The system should prefer quality and availability over mechanically forcing ten different question types.

### Question Bank boundary

The Question Bank remains separate from user history:

```text
FACT DATABASE
      ↓
QUESTION SYSTEM
      ↓
QUESTION BANK
      ↓
QUIZ
      ↓
USER HISTORY
```

The Question Bank stores questions the system owns. User history records what a particular user has already seen.

The bank must never become a bottleneck. If eligible bank questions are insufficient, the system should generate and validate additional questions and then serve them.

### Phase 3 stages

**Phase 3A — Question data model & concept/family model**  
Define question records, provenance, concepts, question families, variants, lifecycle status and future access metadata.

**Phase 3B — Question blueprint/template system**  
Define safe generation patterns for the supported question types and domains.

**Phase 3C — Question generator**  
Generate questions from verified facts and relationships.

**Phase 3D — Distractor generator**  
Construct plausible, factually incorrect, unambiguous options.

**Phase 3E — Quality validator**  
Reject questions that fail factual, linguistic, structural, difficulty or freshness checks.

**Phase 3F — Duplicate/family detection**  
Detect exact duplicates, semantic duplicates and related question families before quiz assembly.

**Phase 3G — Quiz assembler**  
Build valid 10-question quizzes while respecting family, topic, difficulty, freshness and user-history rules.

**Phase 3H — Worker integration**  
Connect the validated Current Affairs question system to the existing Worker without disturbing Science, General Knowledge, Africa & Nigeria or AdMob.

**Phase 3I — Android/debug testing**  
Test Current Affairs end-to-end on the Debug APK, including repeated quizzes, family cooldown behaviour, connectivity failures, stale-fact handling and regression of existing categories.

### Phase 3 implementation safety rule

Do not replace the verified fact database with generated questions. Facts remain the source of truth. Do not mix Question Bank storage with user recent history. Do not reintroduce NewsData into the Current Affairs route.



## 16. V1 implementation record

The V1 Current Affairs implementation was completed for the 24 September 2026 stabilization checkpoint:

- Verified fact-first knowledge base and source/provenance model.
- Question blueprints, deterministic generation, distractors, quality validation, duplicate/family detection and quiz assembly.
- Question Bank management and Worker serving integration.
- Request-scoped runtime recovery when recent-history filtering exhausts the seeded pool.
- Regression testing preserved separation from NewsData and existing categories.
- Recent-history exhaustion testing returned a complete 10-question quiz without re-serving the supplied recent-history IDs.

These are historical V1 implementation facts. They do not mean V2 AI services, D1 persistence or new production endpoints are implemented.
