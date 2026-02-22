# Krikalp — AI-based Web Chat Platform

## Overview

Krikalp is an AI-driven, extensible web chat application that lets users interact with multiple AI models through a single, plug-and-play Node.js backend. The system supports free models out-of-the-box and allows users to add premium model subscriptions. Primary goals are flexibility, extensibility, and a user-focused experience with AI-powered features.

## Technology Stack

- Frontend: Angular
- Backend: Node.js (express or similar)
- Database: SQL Server
- AI integration: Pluggable connectors in Node.js (supports multiple model providers)

## Initial Features

1. Text-based chat (real-time or request/response)
2. Document summarization and text summarization
3. Document review (single and multiple documents)
4. Image generation from prompts
5. Video generation from prompts
6. Dedicated user page with AI-driven suggestions and personalized content (funny cartoons, recommendations)
7. User data form on the dedicated page to gather profile details (interests, profession, birthdays, anniversaries, family details) to generate personalized cards and suggestions

## Architecture & AI Model Integration

- The Node.js backend exposes a model-adapter interface: each AI provider implements the adapter so models are plug-and-play.
- The system can list available models (free and premium) and route requests to the selected adapter.
- Admins or users with subscriptions can add premium model credentials; the backend stores connector configs and secrets in environment variables or a secure vault.
- Supported model types: chat/text, summarization, document analysis, image-generation, video-generation.

## Data & Privacy

- Treat user data carefully: store only what is necessary, encrypt sensitive fields, and follow user consent for data usage.
- When sending user data to external AI providers, surface a clear consent step and document which provider receives what data.

## Installation & Setup (local dev)

Prerequisites:
- Node.js (LTS)
- Angular CLI
- SQL Server (local or remote)

Backend (Node.js):

1. Copy environment example to `.env` and set database and model keys.
2. Install dependencies: `npm install` in the backend folder.
3. Run database migrations / create schema in SQL Server.

Frontend (Angular):

1. `npm install` in the frontend folder.
2. Update environment config to point to the backend API.
3. `ng serve` to run the Angular dev server.

Database:

- Create the application's database in SQL Server and run the provided schema/migrations.

Environment variables (examples):

- `DB_CONNECTION_STRING` – SQL Server connection string
- `PORT` – backend port
- `MODEL_PROVIDER_CONFIG` – metadata or pointers to configured AI adapters

Running the app:

1. Start SQL Server or ensure remote DB is reachable.
2. Start backend: `npm run dev` (or as configured).
3. Start frontend: `ng serve --open`.

## Adding New AI Model Connectors

1. Implement the adapter interface in the backend (methods: chat, summarize, review, imageCreate, videoCreate).
2. Register the adapter in the model registry so it appears in the model selection list.
3. Add configuration options (API keys, endpoints) to the admin UI or `.env`.
4. Ensure secure storage for credentials.

## Contributing

- Open issues for feature requests and bugs.
- Follow the adapter pattern for adding providers.

## License

- Add your preferred license here.

## Contact

- For questions or help, contact the project maintainer.
