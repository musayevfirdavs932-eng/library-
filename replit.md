# Medical Learning Platform

## Overview

A full-stack AI-powered medical learning platform for university students. Students can read medical books, highlight text, and use AI to explain, test, summarize, or create notes from any selected text — all in Uzbek.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (ESM bundle)
- **AI**: Gemini 2.5 Flash via Replit AI Integrations
- **Frontend**: React + Vite + TailwindCSS

## Structure

```text
artifacts/
├── api-server/               # Express API backend
│   └── src/
│       ├── routes/
│       │   ├── auth.ts       # Login, register, logout, me
│       │   ├── books.ts      # Book CRUD + file upload + content
│       │   ├── categories.ts # Category management
│       │   ├── ai.ts         # Explain, Test, Notes, Summarize (Gemini)
│       │   ├── highlights.ts # Text highlight save/delete
│       │   └── notes.ts      # Saved notes CRUD
│       └── lib/
│           └── auth-middleware.ts  # JWT auth middleware
├── medical-learning/         # React + Vite frontend
│   └── src/
│       ├── pages/
│       │   ├── login.tsx     # Login page
│       │   ├── register.tsx  # Register page
│       │   ├── library.tsx   # Book library (home page)
│       │   ├── reader.tsx    # Book reader with AI sidebar
│       │   ├── notes.tsx     # Saved notes history
│       │   └── admin.tsx     # Admin panel (books + categories)
│       ├── components/
│       │   └── layout/Navbar.tsx
│       └── hooks/
│           └── use-auth.tsx
lib/
├── api-spec/openapi.yaml     # API contract (single source of truth)
├── api-client-react/         # Generated React Query hooks
├── api-zod/                  # Generated Zod schemas
├── db/src/schema/            # Drizzle ORM tables
│   ├── users.ts
│   ├── categories.ts
│   ├── books.ts
│   ├── highlights.ts
│   └── notes.ts
└── integrations-gemini-ai/   # Gemini AI client
```

## Demo Credentials

- **Admin**: admin@medlearn.uz / password
- **Student**: student@medlearn.uz / password

## Features

### Students
- Browse book library with search and category filters
- Read books with text selection → AI button
- AI Sidebar with 4 actions (all respond in Uzbek):
  - **Explain** – Simple explanation with real-life examples
  - **Test** – 5-10 MCQ questions with answer checking
  - **Notes** – Bullet-point summary
  - **Summary** – 3-5 sentence summary
- Save highlights and notes from AI responses
- View all saved notes history

### Admin
- Add/delete books with file upload (PDF, DOCX, TXT)
- Categorize books
- Manage book categories

## API Endpoints

All endpoints are prefixed with `/api`:
- `POST /auth/login`, `POST /auth/register`, `GET /auth/me`, `POST /auth/logout`
- `GET /books`, `POST /books`, `GET /books/:id`, `DELETE /books/:id`
- `POST /books/:id/upload`, `GET /books/:id/content`
- `GET /categories`, `POST /categories`
- `POST /ai/explain`, `POST /ai/test`, `POST /ai/notes`, `POST /ai/summarize`
- `GET /highlights`, `POST /highlights`, `DELETE /highlights/:id`
- `GET /notes`, `POST /notes`, `DELETE /notes/:id`

## Database Schema

- `users` – id, email, password_hash, name, role (student/admin)
- `categories` – id, name, description
- `books` – id, title, author, description, category_id, file_type, file_size, file_content
- `highlights` – id, book_id, user_id, selected_text, page_number, color
- `notes` – id, book_id, user_id, title, content, ai_action, source_text
