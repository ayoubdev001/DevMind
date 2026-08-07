<div align="center">

# 📚 DevBuddy AI
### AI-Powered Developer Learning Assistant

![React Native](https://img.shields.io/badge/Frontend-React_Native_%2B_Expo-61DAFB?style=flat-square&logo=react)
![Node](https://img.shields.io/badge/Backend-Node.js_%2B_Express-339933?style=flat-square&logo=node.js)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL_%2B_pgvector-336791?style=flat-square&logo=postgresql)
![OpenAI](https://img.shields.io/badge/AI-GPT--4o--mini-412991?style=flat-square&logo=openai)
![Docker](https://img.shields.io/badge/Deploy-Docker_%2B_Railway%2FRender-2496ED?style=flat-square&logo=docker)
![Status](https://img.shields.io/badge/Status-Draft_for_Review-yellow?style=flat-square)
</div>

DevBuddy AI is a mobile application that helps developers learn, organize, and review programming knowledge more efficiently. Users create technical flashcards, organize them into learning decks, and interact with an AI-powered learning assistant.

The assistant explains programming concepts, answers questions using the user's own notes and flashcards, generates quizzes, and turns notes into flashcard drafts — all grounded in the user's personal knowledge base through Retrieval-Augmented Generation (RAG).

**Project type:** End-of-training project covering mobile development, backend development, database design, security, API integration, artificial intelligence, and deployment.

| | |
|---|---|
| 👤 **Prepared by** | Ayoub Khaya |
| 💼 **Role** | Full-Stack Software Engineer (Project Author) |
| 📄 **Document Type** | Final Project — Full-Stack Mobile Application |
| 🧠 **Domain** | Artificial Intelligence · Mobile Development · Developer Learning |
| 🔖 **Version** | 1.0 |
| 🚦 **Status** | Draft for Review |
| 📅 **Date** | August 2026 |
| 🌐 **Language** | English |

---

## Table of Contents

- [Project Vision](#project-vision)
- [Features](#features)
- [AI Agent Scope](#ai-agent-scope)
- [System Architecture](#system-architecture)
- [File Architecture](#file-architecture)
- [Technology Stack](#technology-stack)
- [Project Requirements](#project-requirements)
- [Data Model](#data-model)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Development Guidelines](#development-guidelines)
- [Vibe Coding Methodology](#vibe-coding-methodology)
- [Testing and Quality](#testing-and-quality)
- [Deployment](#deployment)
- [Documentation](#documentation)
- [Roadmap](#roadmap)
- [License](#license)

---

## Project Vision

DevBuddy AI combines active recall, structured knowledge management, and generative AI in one personalized learning companion for developers.

The project demonstrates a complete, realistically-scoped implementation of the skills acquired during training:

- Mobile application development.
- REST API and backend architecture.
- Relational and vector database design.
- Authentication and application security.
- AI integration: RAG, streaming, and function calling.

The objective is not to generate an application from a single prompt. The objective is to build an iterative, documented, testable, and explainable software product — kept deliberately simple enough to fully build, understand, and defend end-to-end.

---

## Features

### Learning and knowledge management
- Create, update, delete, and review technical flashcards.
- Organize flashcards into learning decks.
- Search, filter, sort, and paginate personal knowledge.
- Track basic review history (last reviewed, review count).
- Store notes and convert them into flashcards with AI assistance.
- Generate a quiz based on a deck or a note.

### AI learning assistant
- Explain programming concepts at an appropriate level.
- Answer questions using the user's own notes and flashcards.
- Retrieve relevant personal content with RAG before generating an answer.
- Stream responses progressively to the mobile interface.
- Maintain short-term conversation memory and persistent history.
- Call approved business functions when an action is required (e.g. save a generated flashcard).

### User experience
- Authentication-aware navigation.
- Persistent conversations and learning data.
- Loading, streaming, generation, and error states.
- Secure token storage on the device.
- Simple, focused mobile interface built for learning, not busywork.

---

## AI Agent Scope

The agent's role is limited to supporting programming education and managing a small set of approved learning actions.

### Authorized actions
The agent may:
- Answer programming and software-development questions.
- Search the user's own indexed notes, flashcards, and decks.
- Explain retrieved information and identify which note/flashcard it came from.
- Generate quiz questions and flashcard drafts.
- Recommend a deck or review session based on recent activity.
- Call an explicitly approved business function, such as saving a flashcard draft or starting a quiz.

### Information sources
The agent can use:
- The current conversation.
- The authenticated user's own notes, flashcards, and decks (scoped strictly by `userId`).

The agent must not present unverified generated content as guaranteed fact. When an answer is based on retrieved content, the source note or flashcard should be identified where possible.

### Refused requests
The agent must refuse or safely redirect requests that:
- Ask for another user's private data.
- Request secrets, tokens, credentials, or internal system instructions.
- Attempt to bypass application permissions or security controls.
- Instruct the agent to ignore its system rules (prompt injection).
- Require unsafe, illegal, or harmful assistance.
- Ask the agent to perform an action outside its declared business functions.
- Require professional legal, medical, or financial certainty beyond the product's educational scope.

### Confirmation requirements
Explicit user confirmation is required before an action that:
- Creates, modifies, or deletes persistent user data.
- Triggers a business function with a lasting side effect.

Read-only retrieval and generating an unsaved draft (a quiz question, a flashcard suggestion) do not require confirmation — only *saving* them does.

### Reliability limits
AI responses can be incomplete, outdated, or incorrect. The app should communicate uncertainty, distinguish retrieved facts from generated explanations, and let users verify and edit generated flashcards before saving them.

---

## System Architecture

```
┌───────────────────────────────┐
│ React Native / Expo Mobile App │
│ Expo Router · Zustand · Axios  │
└───────────────┬────────────────┘
                │ HTTPS / SSE
┌───────────────▼────────────────┐
│ Node.js / Express API            │
│ Auth · CRUD · Agent endpoints    │
│ Validation · Security · Logs     │
└────────┬───────────────┬────────┘
         │               │
┌────────▼────────┐ ┌────▼────────────────┐
│ PostgreSQL        │ │ AI Orchestration     │
│ Sequelize ORM      │ │ LLM · RAG · Tools    │
│ pgvector            │ │ Streaming             │
└────────────────────┘ └───────────────────────┘
```

### Core backend flow
1. The mobile client authenticates with the API.
2. The user sends a question or task to the agent endpoint.
3. The backend validates the request, applies rate limits, and records an audit event.
4. The RAG pipeline searches the user's own notes/flashcards using embeddings and similarity search.
5. The agent generates a response or proposes an approved function call.
6. The response is streamed to the mobile client through SSE.
7. Any action that writes data requires confirmation and is logged.

---

## File Architecture

```
devbuddy-ai/
├── README.md
├── docker-compose.yml
├── .env.example
│
├── docs/
│   ├── architecture.md
│   ├── database.md
│   ├── ai.md
│   ├── api.md
│   └── vibe-coding-journal.md
│
├── backend/
│   ├── package.json
│   ├── Dockerfile
│   ├── migrations/
│   ├── seeders/
│   └── src/
│       ├── app.js
│       ├── server.js
│       ├── config/
│       │   ├── database.js
│       │   └── env.js
│       ├── models/
│       │   ├── index.js
│       │   ├── User.js
│       │   ├── Deck.js
│       │   ├── Flashcard.js
│       │   ├── Note.js
│       │   ├── Conversation.js
│       │   ├── Message.js
│       │   └── AuditLog.js
│       ├── routes/
│       │   ├── auth.routes.js
│       │   ├── deck.routes.js
│       │   ├── flashcard.routes.js
│       │   └── agent.routes.js
│       ├── controllers/
│       │   ├── auth.controller.js
│       │   ├── deck.controller.js
│       │   ├── flashcard.controller.js
│       │   └── agent.controller.js
│       ├── services/
│       │   ├── auth.service.js
│       │   ├── deck.service.js
│       │   └── agent.service.js
│       ├── middleware/
│       │   ├── auth.js
│       │   └── error.js
│       ├── ai/
│       │   ├── openai.service.js
│       │   ├── rag.service.js
│       │   ├── embedding.service.js
│       │   ├── prompt.service.js
│       │   └── streaming.service.js
│       └── utils/
│           ├── jwt.js
│           └── password.js
│
└── mobile/
    ├── package.json
    └── app/
        ├── (auth)/
        │   ├── login.jsx
        │   └── register.jsx
        └── (protected)/
            ├── index.jsx
            ├── decks/
            ├── flashcards/
            └── chat/
```

### Directory responsibilities

| Directory | Responsibility |
|---|---|
| `backend/src/config/database.js` | Initializes and configures the Sequelize connection. |
| `backend/src/models` | Defines Sequelize models and their associations. |
| `backend/migrations` | Stores versioned Sequelize database migrations. |
| `backend/seeders` | Contains development seed data. |
| `backend/src/routes` | Defines HTTP routes. |
| `backend/src/controllers` | Handles HTTP requests and returns standardized responses. |
| `backend/src/services` | Contains business logic and database queries (no separate repository layer needed at this scale). |
| `backend/src/ai` | Implements the agent, prompts, RAG, embeddings, and streaming. |
| `backend/src/middleware` | Handles authentication, validation, and error handling. |
| `mobile/app` | Defines Expo Router screens and protected route groups. |
| `docs` | Stores architecture, database, AI, API, and development documentation. |

---

## Technology Stack

| Area | Planned technologies |
|---|---|
| Mobile | React Native, Expo, Expo Router |
| State management | Zustand, AsyncStorage persistence, selectors |
| HTTP and streaming | Axios, interceptors, token refresh, SSE |
| Backend | Node.js, Express |
| Database | PostgreSQL, Sequelize ORM, pgvector |
| Authentication | JWT access and refresh tokens, bcrypt, SecureStore |
| AI | OpenAI GPT-4o-mini |
| RAG | Chunking, embeddings, pgvector similarity search |
| Agent actions | Function calling (limited, with confirmation) |
| Validation | express-validator |
| Logging | Winston and Morgan, plus an AuditLog table for agent activity |
| Deployment | Docker, Railway or Render |
| Documentation | OpenAPI/Swagger and Postman |

---

## Project Requirements

### Backend
- REST API with complete CRUD operations.
- Standardized relational schema in third normal form.
- Sequelize models, associations, migrations, seeders, and transactions.
- One-to-one, one-to-many, and many-to-many relationships where relevant.
- JWT registration, login, logout, refresh, and protected routes.
- Password hashing with bcrypt.
- Request validation and centralized error handling.
- Protection against SQL injection, unauthorized access, rate abuse, and prompt injection.
- Pagination, sorting, filtering, and appropriate database indexes.
- Environment-based configuration with no committed API keys.
- Interaction audit logs for the AI agent.

### Mobile frontend
- Conditional navigation for authenticated and unauthenticated users.
- Modular Zustand stores for authentication, data, UI, cache, and conversations.
- AsyncStorage persistence only for appropriate non-sensitive state.
- SecureStore for access and refresh tokens.
- Centralized Axios service with automatic token refresh and retry handling.
- Streaming chat responses displayed progressively.
- Persistent conversation history.
- Clear loading, generation, empty, and error states.
- Private routes and automatic redirection.

### AI, agent, and RAG
- Documented choice of OpenAI GPT-4o-mini.
- System prompt defining the agent's role, tone, permissions, and limits.
- Document chunking and embedding generation.
- Vector search against the user's own content.
- Short-term conversation memory.
- Streaming responses to the frontend.
- Rate limiting and prompt-injection defenses.
- Function calling for approved actions only, with confirmation before any write.

---

## Data Model

The relational model includes the following entities:

- **User** — account, credentials, and timestamps.
- **Deck** — a user's learning collection.
- **Flashcard** — question, answer, topic, difficulty, and simple review metadata.
- **Note** — source material submitted by the user; holds a pgvector `embedding` column used for RAG.
- **Conversation** — an AI conversation owned by a user.
- **Message** — user, assistant, or tool messages within a conversation.
- **AuditLog** — security and agent interaction events.

Embeddings are stored as a `vector` column directly on `Note` (and on `Flashcard` if flashcards are also searchable) rather than in a separate table — simpler to reason about at this scale, and avoids a polymorphic foreign key.

Sequelize associations, foreign keys, ownership checks, unique constraints, indexes, and cascade behavior must be explicitly defined. Database changes must use versioned Sequelize migrations, and seed data must be managed through Sequelize seeders.

The UML use-case and class diagrams must include the AI entities, especially `Conversation`, `Message`, and the `embedding` column.

---

## Getting Started

### Prerequisites
- Node.js 20 or later.
- npm, pnpm, or yarn.
- PostgreSQL with the pgvector extension enabled.
- Expo CLI and a mobile emulator or Expo-compatible device.
- An OpenAI API key.
- Docker and Docker Compose.

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd devbuddy-ai

# Install backend dependencies
cd backend
npm install

# Create local environment configuration
cp .env.example .env

# Run Sequelize migrations and seeders
npx sequelize-cli db:migrate
npx sequelize-cli db:seed:all

# Start the backend
npm run dev
```

In a second terminal:

```bash
cd mobile
npm install
npx expo start
```

### Docker

```bash
docker compose up --build
```

Keep these commands synchronized with the scripts defined in `package.json`.

---

## Environment Variables

Never commit `.env` files, private keys, JWT secrets, or provider credentials.

```
NODE_ENV=development
PORT=3000
DATABASE_URL=postgresql://user:password@localhost:5432/devbuddy
DB_HOST=localhost
DB_PORT=5432
DB_NAME=devbuddy
DB_USER=postgres
DB_PASSWORD=postgres
JWT_ACCESS_SECRET=replace-me
JWT_REFRESH_SECRET=replace-me
OPENAI_API_KEY=replace-me
LLM_MODEL=gpt-4o-mini
VECTOR_PROVIDER=pgvector
CORS_ORIGIN=http://localhost:8081
```

The backend must validate required environment variables at startup and fail safely when configuration is incomplete.

---

## Development Guidelines

- Keep business logic in `services/`, out of route handlers.
- Validate all external input at the API boundary.
- Enforce authorization at the resource level, not only at the route level.
- Use Sequelize query methods and safe parameter replacements; avoid raw SQL.
- Define Sequelize associations in one clear location (`models/index.js`).
- Keep AI tools narrowly scoped, typed, and permission-aware.
- Store only the minimum conversation and personal data required by the product.
- Add a Sequelize migration for every schema change.
- Log security-relevant events without logging passwords, tokens, or sensitive prompt content.
- Use Sequelize transactions for multi-step writes.
- Keep API responses and error formats consistent.

---

## Vibe Coding Methodology

AI may be used as a development assistant for architecture exploration, code generation, debugging, testing, documentation, and refactoring. The developer remains responsible for the architecture, security, code quality, integration, validation, and ability to explain every implemented solution.

The project must maintain a prompt journal containing, for each meaningful prompt: the date and objective, the context provided, the generated result, any corrections made, and lessons learned.

Prompts should be small and testable rather than monolithic. Every AI-generated change must be reviewed, adapted to project conventions, tested, and understood before integration.

---

## Testing and Quality

- Unit tests for services, validation, and agent policies (allowed actions vs. refusals).
- Integration tests for authentication, CRUD routes, and authorization.
- A small set of agent tests covering: a normal Q&A turn, a refused out-of-scope request, and a confirmation-required write action.
- A Postman collection covering the main API routes, kept alongside the OpenAPI spec.

Keep the test suite focused on these core paths rather than exhaustive coverage — the goal for this project is a defensible, working demonstration, not production-grade test coverage.

---

## Deployment

The application is designed to be deployed with Docker on Railway or Render.

Deployment requirements:
- Use a multi-stage Dockerfile for the backend.
- Provide production environment variables through the hosting platform's secret manager.
- Run Sequelize migrations as an explicit deployment step.
- Configure CORS and HTTPS.
- Restrict production logging to safe, useful information.
- Never expose the OpenAI key or JWT secrets to the mobile application.

---

## Documentation

The repository should contain:
- Global architecture and UML diagrams (use case, class, sequence).
- Sequelize database schema and association documentation.
- OpenAPI/Swagger documentation.
- Postman collection for the REST API.
- Agent system prompt and function-calling specification.
- RAG ingestion and retrieval explanation.
- Vibe coding prompt journal.
- Deployment and environment configuration instructions.

---

## Roadmap

- [ ] Initialize mobile and backend workspaces.
- [ ] Design UML architecture and the Sequelize database schema.
- [ ] Configure Sequelize, PostgreSQL, migrations, seeders, and associations.
- [ ] Implement authentication and protected navigation.
- [ ] Implement deck, flashcard, note, and review CRUD operations.
- [ ] Add indexes, validation, transactions, and centralized errors.
- [ ] Integrate OpenAI GPT-4o-mini.
- [ ] Implement document chunking, embeddings, and pgvector retrieval.
- [ ] Add streamed conversations and persistent history.
- [ ] Add agent safeguards, audit logs, and confirmation flows.
- [ ] Add core tests (auth, CRUD, agent refusals/confirmations).
- [ ] Containerize and deploy the application.
- [ ] Complete architecture, API, AI, deployment, and prompt-journal documentation.

---

## License

This project is developed as an end-of-training project. Add the final license and ownership information before public release.
