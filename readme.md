<div align="center">

# 📚 DevBuddy AI

## AI-Powered Developer Learning Assistant

![React Native](https://img.shields.io/badge/Frontend-React_Native_%2B_Expo-61DAFB?style=flat-square&logo=react)
![Node](https://img.shields.io/badge/Backend-Node.js_%2B_Express-339933?style=flat-square&logo=node.js)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL_%2B_pgvector-336791?style=flat-square&logo=postgresql)
![Sequelize](https://img.shields.io/badge/ORM-Sequelize-52B0E7?style=flat-square&logo=sequelize)
![OpenAI](https://img.shields.io/badge/AI-GPT--4o--mini-412991?style=flat-square&logo=openai)
![Docker](https://img.shields.io/badge/Deploy-Docker_%2B_Railway%2FRender-2496ED?style=flat-square&logo=docker)
![Status](https://img.shields.io/badge/Status-Draft_for_Review-yellow?style=flat-square)

</div>

DevBuddy AI is a mobile application that helps developers learn, organize, and review programming knowledge more efficiently. Users can create technical flashcards, organize them into learning decks, and interact with an AI-powered learning assistant.

The assistant can explain programming concepts, answer questions using the user's personal knowledge base, generate quizzes, and transform notes into flashcards through Retrieval-Augmented Generation (RAG).

> **Project type:** End-of-training project covering mobile development, backend development, database design, security, API integration, artificial intelligence, and deployment.

<br>

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

The project demonstrates a complete implementation of the skills acquired during training:

- Mobile application development.
- REST API and backend architecture.
- Relational and vector database design.
- Authentication and application security.
- AI integration, RAG, streaming, and function calling.
- External tool integration through MCP.
- Automated workflows and cloud deployment.

The objective is not to generate an application from a single prompt. The objective is to build an iterative, documented, testable, and explainable software product.

---

## Features

### Learning and knowledge management

- Create, update, delete, and review technical flashcards.
- Organize flashcards into learning decks.
- Search, filter, sort, and paginate personal knowledge.
- Track learning progress and review history.
- Store notes and convert them into flashcards with AI assistance.
- Generate quizzes based on a deck, topic, or personal notes.

### AI learning assistant

- Explain programming concepts at an appropriate level.
- Answer questions using the user's personal knowledge base.
- Retrieve relevant documents with RAG before generating an answer.
- Stream responses progressively to the mobile interface.
- Maintain short-term conversation memory and persistent history.
- Recommend learning resources or the next review action.
- Call approved business functions when an action is required.

### User experience

- Authentication-aware navigation.
- Persistent conversations and learning data.
- Loading, streaming, generation, and error states.
- Secure token storage on the device.
- Responsive mobile interface built for focused learning.

---

## AI Agent Scope

The agent's role is limited to supporting programming education and managing approved learning workflows.

### Authorized actions

The agent may:

- Answer programming and software-development questions.
- Search the user's indexed notes, flashcards, and decks.
- Explain retrieved information and identify relevant sources.
- Generate quizzes and flashcard drafts.
- Recommend a deck, review session, or learning resource.
- Call explicitly approved business functions, such as creating a flashcard draft or starting a quiz.
- Use an MCP tool when that tool is documented, available, and relevant to the request.

### Information sources

The agent can use:

- The current conversation.
- The authenticated user's personal knowledge base.
- Approved application documentation or a documentary database.
- Explicitly connected external services and MCP tools.

The agent must not present unverified generated content as guaranteed fact. When the answer is based on retrieved content, the relevant source should be identified where possible.

### Refused requests

The agent must refuse or safely redirect requests that:

- Ask for another user's private data or unauthorized access.
- Request secrets, tokens, credentials, or internal system instructions.
- Attempt to bypass application permissions or security controls.
- Instruct the agent to ignore its system rules through prompt injection.
- Require unsafe, illegal, discriminatory, or harmful assistance.
- Ask the agent to perform an action outside its declared business functions.
- Require professional legal, medical, or financial certainty beyond the product's educational scope.

### Confirmation requirements

Explicit user confirmation is required before an action that:

- Creates, modifies, or deletes persistent user data.
- Sends information to an external service.
- Triggers an external tool or business action with side effects.
- Starts an automation or notification that affects the user's account.

Read-only retrieval and generation of an unsaved draft may be performed without confirmation.

### Reliability limits

AI responses can be incomplete, outdated, or incorrect. The application should communicate uncertainty, distinguish retrieved facts from generated explanations, and allow users to verify and edit generated flashcards before saving them.

---

## System Architecture

The planned architecture is divided into the following layers:

```text
┌───────────────────────────────┐
│ React Native / Expo Mobile App│
│ Expo Router · Zustand · Axios │
└───────────────┬───────────────┘
                │ HTTPS / SSE
┌───────────────▼───────────────┐
│ Node.js / Express API          │
│ Auth · CRUD · Agent endpoints  │
│ Validation · Security · Logs   │
└────────┬──────────────┬───────┘
         │              │
┌────────▼────────┐ ┌───▼────────────────┐
│ PostgreSQL       │ │ AI Orchestration   │
│ Sequelize ORM    │ │ LLM · RAG · Tools  │
│ pgvector         │ │ Streaming · MCP    │
└──────────────────┘ └──────────┬─────────┘
                                │
                         ┌──────▼──────┐
                         │ MCP servers │
                         │ n8n workflows│
                         │ External APIs│
                         └─────────────┘
```

### Core backend flow

1. The mobile client authenticates with the API.
2. The user sends a question or task to the agent endpoint.
3. The backend validates the request, applies rate limits, and records an audit event.
4. The RAG pipeline searches authorized knowledge using embeddings and similarity search.
5. The agent generates a response or proposes an approved function call.
6. The response is streamed to the mobile client through SSE.
7. Side-effecting actions require confirmation and are logged.

---

## File Architecture

The repository is organized into separate mobile, backend, database, AI, integration, testing, and documentation layers.

```text
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
│
│   ├── migrations/
│   ├── seeders/
│
│   └── src/
│       ├── app.js
│       ├── server.js
│
│       ├── config/
│       │   ├── database.js
│       │   └── env.js
│
│       ├── models/
│       │   ├── index.js
│       │   ├── User.js
│       │   ├── Deck.js
│       │   ├── Flashcard.js
│       │   ├── Conversation.js
│       │   ├── Message.js
│       │   └── Embedding.js
│
│       ├── routes/
│       │   ├── auth.routes.js
│       │   ├── deck.routes.js
│       │   ├── flashcard.routes.js
│       │   └── agent.routes.js
│
│       ├── controllers/
│       │   ├── auth.controller.js
│       │   ├── deck.controller.js
│       │   ├── flashcard.controller.js
│       │   └── agent.controller.js
│
│       ├── services/
│       │   ├── auth.service.js
│       │   ├── deck.service.js
│       │   └── agent.service.js
│
│       ├── middleware/
│       │   ├── auth.js
│       │   └── error.js
│
│       ├── ai/
│       │   ├── openai.service.js
│       │   ├── rag.service.js
│       │   ├── embedding.service.js
│       │   ├── prompt.service.js
│       │   └── streaming.service.js
│
│       └── utils/
│           ├── jwt.js
│           └── password.js
│
├── mobile/

│   ├── package.json
│
│   ├── app/
│   │
│   ├── (auth)/
│   │   ├── login.jsx
│   │   └── register.jsx
│   │
│   └── (protected)/
│       ├── index.jsx
│       ├── decks/
│       ├── flashcards/
│       └── chat/
│
│
└── .github/
    └── workflows/
        └── ci.yml
```

### Directory responsibilities

| Directory | Responsibility |
|---|---|
| `backend/src/config/database.ts` | Initializes and configures the Sequelize connection. |
| `backend/src/models` | Defines Sequelize models and their relationships. |
| `backend/src/migrations` | Stores versioned Sequelize database migrations. |
| `backend/src/seeders` | Contains development and test seed data. |
| `backend/src/routes` | Defines versioned HTTP routes. |
| `backend/src/controllers` | Handles HTTP requests and returns standardized responses. |
| `backend/src/services` | Contains application and business logic. |
| `backend/src/repositories` | Encapsulates database queries and persistence logic. |
| `backend/src/ai` | Implements the agent, prompts, RAG, embeddings, tools, and streaming. |
| `backend/src/integrations` | Connects the application to the LLM, MCP, and n8n. |
| `backend/src/middlewares` | Handles authentication, authorization, validation, rate limiting, and errors. |
| `mobile/app` | Defines Expo Router screens and protected route groups. |
| `mobile/src/features` | Groups mobile UI and logic by product feature. |
| `mobile/src/stores` | Contains modular Zustand stores and persisted client state. |
| `mobile/src/services` | Centralizes Axios, authentication, token refresh, and SSE. |
| `docs` | Stores architecture, database, AI, API, deployment, and development documentation. |

---

## Technology Stack

| Area | Planned technologies |
|---|---|
| Mobile | React Native, Expo, Expo Router |
| State management | Zustand, AsyncStorage persistence, selectors |
| HTTP and streaming | Axios, interceptors, token refresh, SSE |
| Backend | Node.js, Express, MVC or Clean Architecture |
| Database | PostgreSQL, Sequelize ORM, pgvector |
| Authentication | JWT access and refresh tokens, bcrypt, SecureStore |
| AI | OpenAI GPT-4o-mini; Ollama may be used locally |
| RAG | Chunking, embeddings, pgvector similarity search |
| Agent tools | Function calling and MCP |
| Validation | Zod, Joi, or express-validator |
| Logging | Winston and Morgan, including agent audit logs |
| Automation | n8n (bonus) |
| Deployment | Docker, Railway or Render |
| Documentation | OpenAPI/Swagger and Postman |

---

## Project Requirements

### Backend

- REST API with versioned routes and complete CRUD operations.
- Standardized relational schema in third normal form.
- MVC or Clean Architecture boundaries.
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
- Relevant use of Expo APIs, permissions, and Expo Updates where applicable.

### AI, agent, and RAG

- Documented choice of OpenAI GPT-4o-mini.
- System prompt defining the agent's role, tone, permissions, and limits.
- Document chunking and embedding generation.
- Vector search against authorized user content.
- Conversation history and short-term memory management.
- Streaming responses to the frontend.
- Moderation, rate limiting, and prompt-injection defenses.
- Function calling for approved actions only.

### MCP bonus

- Connect the agent to at least one MCP server, or build a small MCP server exposing a business action.
- Document the client/server architecture and available tools.
- Prepare a reproducible MCP demonstration scenario for the project defense.

### Automation bonus

Document an AI workflow for a recurring task, such as periodic learning summaries, flashcard classification, or personalized review notifications. Document its trigger, processing steps, safeguards, and output.

---

## Data Model

The relational model includes the following entities:

- `User`: account, credentials, preferences, and timestamps.
- `Deck`: a user's learning collection.
- `Flashcard`: question, answer, topic, difficulty, and review metadata.
- `Note`: source material submitted by the user.
- `Conversation`: an AI conversation owned by a user.
- `Message`: user, assistant, tool, or system messages.
- `Embedding`: vector representation of indexed content.
- `Review`: learning activity and progress information.
- `AuditLog`: security and agent interaction events.

Sequelize associations, foreign keys, ownership checks, unique constraints, indexes, and cascade behavior must be explicitly defined. Database changes must use versioned Sequelize migrations, and seed data must be managed through Sequelize seeders.

The UML use-case and class diagrams must include the AI entities, especially `Conversation`, `Message`, and `Embedding`.

---

## Getting Started

### Prerequisites

- Node.js 20 or later.
- npm, pnpm, or yarn.
- PostgreSQL with the `pgvector` extension enabled for RAG.
- Expo CLI and a mobile emulator or Expo-compatible device.
- An OpenAI API key, unless using a local model.
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

```env
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

- Use feature-based modules and keep business logic outside route handlers.
- Validate all external input at the API boundary.
- Enforce authorization at the resource level, not only at the route level.
- Use Sequelize query methods and safe parameter replacements; avoid unsafe raw SQL.
- Define Sequelize associations in one clear location.
- Keep AI tools narrowly scoped, typed, and permission-aware.
- Store only the minimum conversation and personal data required by the product.
- Add a Sequelize migration for every schema change.
- Log security-relevant events without logging passwords, tokens, or sensitive prompt content.
- Use Sequelize transactions for multi-step writes.
- Keep API responses and error formats consistent.

---

## Vibe Coding Methodology

AI may be used as a development assistant for architecture exploration, code generation, debugging, testing, documentation, and refactoring. The developer remains responsible for the architecture, security, code quality, integration, validation, and ability to explain every implemented solution.

The project must maintain a prompt journal containing the date and objective of each prompt, the context provided, the generated result, corrections, rejected suggestions, validation tests, and lessons learned.

Prompts should be small and testable rather than monolithic. Every AI-generated change must be reviewed, adapted to project conventions, tested, and understood before integration.

---

## Testing and Quality

The project should include:

- Unit tests for services, utilities, validation, and agent policies.
- Integration tests for authentication, CRUD routes, Sequelize database operations, and authorization.
- Agent tests for allowed actions, refusals, prompt-injection attempts, and confirmation flows.
- RAG tests for document ingestion, retrieval relevance, and unauthorized-content isolation.
- Mobile tests for navigation guards, token refresh, chat streaming, and error states.
- API collection tests using Postman or an equivalent tool.
- Linting, formatting, and type checking in the development workflow.

Quality checks should run automatically in CI before deployment.

---

## Deployment

The application is designed to be deployed with Docker on Railway or Render.

Deployment requirements:

- Use optimized multi-stage Dockerfiles where appropriate.
- Provide production environment variables through the hosting platform's secret manager.
- Run Sequelize migrations as an explicit deployment step.
- Configure CORS, HTTPS, health checks, and database backups.
- Restrict production logging to safe, useful information.
- Never expose OpenAI keys or JWT secrets to the mobile application.
- Document rollback and migration procedures.

---

## Documentation

The repository should contain:

- Global architecture and UML diagrams.
- Use-case and class diagrams including AI entities.
- Sequelize database schema, associations, and migration documentation.
- OpenAPI/Swagger documentation.
- Postman collection for the REST API.
- Agent system prompt and tool specifications.
- RAG ingestion and retrieval explanation.
- MCP client/server documentation and demo scenario.
- Automation workflow documentation, if implemented.
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
- [ ] Integrate an MCP server or business tool.
- [ ] Add automated tests and CI checks.
- [ ] Containerize and deploy the application.
- [ ] Complete the architecture, API, AI, deployment, and prompt-journal documentation.

---

## License

This project is developed as an end-of-training project. Add the final license and ownership information before public release.
