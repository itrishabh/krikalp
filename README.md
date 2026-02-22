# Krikalp — AI-Based Web Chat Platform (Architecture Blueprint)

## 1) Product Vision

Krikalp is a **modular AI web application** where users can chat, summarize/review documents, and generate media (images/videos) from one place.

Key principle: **plug-and-play AI provider integration** on Node.js so any supported model can be selected dynamically at runtime.

The platform should support:
- Free/public models by default
- User-owned premium model subscriptions (bring-your-own-key)
- Feature-aware model routing (text, document, image, video)

---

## 2) Technology Stack

- **Frontend**: Angular (modular feature architecture + reactive forms + RxJS)
- **Backend**: Node.js + Express/Nest-style service layering
- **Database**: SQL Server
- **AI Layer**: Provider Adapter + Model Registry + Capability Router

---

## 3) Core Feature Scope (Phase 1)

1. **Text Chat**
   - Multi-model chat sessions
   - Conversation history
   - Model switch per conversation/new message (configurable)

2. **Document & Text Summarization**
   - Plain text summarization
   - Single document summarization (PDF, DOCX, TXT)

3. **Document Review**
   - Single document review (quality, tone, risks, grammar, compliance)
   - Multi-document comparative review (similarities, differences, action items)

4. **Image Generation by Prompt**
   - Prompt + style presets
   - Output gallery in user workspace

5. **Video Generation by Prompt**
   - Prompt + duration/style preferences
   - Async job pipeline with progress status

6. **Dedicated "AI Growth" User Page**
   - Personalized suggestions on using AI better
   - Insights based on user behavior and selected profile data
   - Auto-funny cartoon ideas/content generation

7. **Personal Data Form (Dedicated Page)**
   - Fields: interests, profession, birthday, anniversary, spouse name, children names/birthdays, etc.
   - Use this data for personalized suggestions/cards/reminders

---

## 4) High-Level Architecture

```text
Angular Web App
  ├─ Auth + Profile Module
  ├─ Chat Module
  ├─ Document Intelligence Module
  ├─ Media Generation Module
  └─ AI Growth Module

Node.js API Layer
  ├─ Auth & User Service
  ├─ Conversation Service
  ├─ Document Service
  ├─ Media Service (Image/Video)
  ├─ Recommendation Service
  ├─ Model Registry Service
  ├─ AI Routing Service
  └─ Provider Adapter Layer
        ├─ OpenAI Adapter
        ├─ Gemini Adapter
        ├─ Open-source Adapter (local/hosted)
        └─ Other adapters (plug-in)

SQL Server
  ├─ Users / Profiles / Consents
  ├─ Model Config / Provider Keys (encrypted references)
  ├─ Conversations / Messages
  ├─ Documents / Reviews / Summaries
  ├─ Media Jobs / Media Assets
  └─ Recommendation Logs
```

---

## 5) Dynamic AI Model System (Plug-and-Play)

### 5.1 Adapter Contract

Create a standard interface every provider must implement:

- `chat(request)`
- `summarize(request)`
- `reviewDocument(request)`
- `generateImage(request)`
- `generateVideo(request)`
- `getCapabilities()`
- `validateCredentials()`

### 5.2 Model Registry

Maintain a registry table/config for all models:
- Model name
- Provider
- Capabilities (chat/summarize/image/video)
- Free or premium
- Cost metadata (optional)
- Active/inactive state

### 5.3 Runtime Routing

When user submits a request:
1. Read selected model
2. Verify capability supports requested feature
3. Resolve credentials (system key or user key)
4. Route request through adapter
5. Persist response metadata + usage

### 5.4 Free + Premium Strategy

- **Free models**: preconfigured by platform admin
- **Premium models**: users add their API keys/subscriptions
- Credentials stored securely (encrypted at rest, secret vault preferred)

---

## 6) Suggested SQL Server Data Model

Core tables (minimum):

- `Users`
- `UserProfiles`
- `UserConsents`
- `ProviderConfigs`
- `UserProviderSubscriptions`
- `ModelCatalog`
- `Conversations`
- `Messages`
- `Documents`
- `DocumentSummaries`
- `DocumentReviews`
- `MediaGenerationJobs`
- `MediaAssets`
- `UserRecommendations`
- `UserEvents` (behavior tracking for suggestion engine)

Security-related:
- `ApiKeyVaultRefs` (store secure references, avoid raw secrets where possible)
- `AuditLogs`

---

## 7) Angular Module Design

- `core/` (auth guards, interceptors, shared services)
- `features/chat/`
- `features/doc-intelligence/`
- `features/media/`
- `features/ai-growth/`
- `features/profile/`
- `shared/ui/`

### Key UI Screens
- Login/Register
- Model selection panel
- Chat workspace
- Document upload + summarize/review results
- Image/video generation workspace
- AI Growth dashboard
- Profile & personal details form

---

## 8) Backend API Design (Example)

- `POST /api/chat/send`
- `POST /api/text/summarize`
- `POST /api/documents/upload`
- `POST /api/documents/:id/summarize`
- `POST /api/documents/:id/review`
- `POST /api/documents/review-multi`
- `POST /api/media/image/generate`
- `POST /api/media/video/generate`
- `GET /api/media/jobs/:jobId`
- `GET /api/models`
- `POST /api/models/user-subscriptions`
- `GET /api/ai-growth/suggestions`
- `POST /api/profile/personal-details`
- `POST /api/cards/generate`

---

## 9) Recommendation Engine for Dedicated User Page

Inputs:
- Profile details (interest/profession/family events)
- Platform usage behavior (what user asks, preferred workflows)
- Optional explicit goals (career, learning, business, content creation)

Outputs:
- "How to use AI better" weekly/daily tips
- Task automations personalized to user role
- Event-driven content suggestions (birthday/anniversary cards)
- Fun cartoon prompts/storyboards based on user profile

Safety controls:
- Explicit consent before using personal/family data
- Data minimization + opt-out per feature

---

## 10) Security, Privacy, Compliance (Must-Have)

- Encrypt sensitive user/profile data at rest
- Never expose premium API keys to frontend
- Use server-side key resolution only
- Add role-based access control (user/admin)
- Consent logs for personalized processing
- Deletion/export workflows for user data
- Provider-level data handling disclosures

---

## 11) Development Roadmap

### Phase A — Foundation
- Auth, user profile, model catalog, chat MVP
- Adapter framework + 2–3 providers

### Phase B — Document Intelligence
- Upload pipeline + summarize/review
- Multi-document comparison

### Phase C — Media
- Image generation
- Async video generation jobs + status UI

### Phase D — AI Growth Personalization
- Personal details form + consent
- Suggestion engine + card/cartoon generation

### Phase E — Production Hardening
- Monitoring, rate limits, audit trails, billing insights

---

## 12) Non-Functional Requirements

- Scalable async processing for heavy AI tasks (video/doc parsing)
- Robust retry and timeout handling per provider
- Provider failover strategy (fallback model)
- Observability: request tracing, latency, token usage, error rates

---

## 13) Local Setup (Planning Baseline)

Prerequisites:
- Node.js LTS
- Angular CLI
- SQL Server

High-level steps:
1. Configure `.env` for DB + provider credentials
2. Run SQL migrations
3. Start backend API
4. Start Angular app
5. Validate model registry and feature routing

Example env keys:
- `DB_CONNECTION_STRING`
- `PORT`
- `JWT_SECRET`
- `ENCRYPTION_KEY`
- `DEFAULT_MODEL`
- `PROVIDER_*` (per adapter)

---

## 14) Suggested Next Build Task

If you want implementation next, start with this order:
1. Build **Model Registry + Adapter Interface** in Node.js
2. Build **Chat API + Angular chat UI**
3. Add **document summarize/review**
4. Add **image/video generation pipeline**
5. Add **AI Growth page + personal form + recommendation service**

This gives you a clean architecture where adding any new AI model remains simple and low-risk.
