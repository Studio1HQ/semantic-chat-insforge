<div align="center">

<img src="https://raw.githubusercontent.com/Studio1HQ/Studio1HQ/main/assets/banners/semantic-chat-insforge.webp" alt="Semantic Chat with InsForge: Chat rooms with real-time messages and meaning-based search" width="100%" />

<br />
<br />

# Semantic Chat with InsForge

### Chat rooms with real-time messages and meaning-based search

A Next.js chat app on InsForge. Messages are embedded with Gemini, stored in InsForge Postgres with pgvector, delivered over InsForge realtime, and searchable by meaning within a room.

<br />

[![Built by Studio1](https://img.shields.io/badge/Built_by_Studio1-f97316?style=for-the-badge&logo=data%3Aimage%2Fsvg%2Bxml%3Bbase64%2CPHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxNjAgMTYwIiBmaWxsPSJub25lIj48cGF0aCBmaWxsLXJ1bGU9ImV2ZW5vZGQiIGNsaXAtcnVsZT0iZXZlbm9kZCIgZD0iTTAgMzVDMCAxNi42ODA3IDE0LjA3NDQgMS42NDg0NCAzMiAwLjEyNjk1M1YxNDFINTEuMDUyM0M1Mi40MzA4IDEzNy4zNDQgNTQuNzQ0NiAxMzMuNjg5IDU3Ljk5MzggMTMwLjAzM0M2MS4xNDQ2IDEyNi41MDggNjQuNjg5MiAxMjMuNDQgNjguNjI3NyAxMjAuODI5QzcyLjU2NjIgMTE4LjM0OSA3Ni4zNTY5IDExNi43ODIgODAgMTE2LjEyOVY4Ni43NTQ0Qzc1LjQ3MDggODcuOTI5MiA3MS4zODQ2IDg5LjY5MTkgNjcuNzQxNSA5Mi4wNDE1QzY0IDk0LjUyMjUgNjAuNzUwOCA5Ny40NiA1Ny45OTM4IDEwMC44NTRDNTYuNTM0NSAxMDIuNzIgNTUuMjU0NSAxMDQuNjU5IDU0LjE1MzkgMTA2LjY3MVYwSDEyNUMxNDQuMzMgMCAxNjAgMTUuNjY5OSAxNjAgMzVWMTI1QzE2MCAxNDQuMzMgMTQ0LjMzIDE2MCAxMjUgMTYwSDEyNFYxOEgxMDQuOTQ4QzEwMy41NjkgMjEuNjgxNiAxMDEuMjU1IDI1LjM2MjggOTguMDA2MiAyOS4wNDQ0Qzk0Ljg1NTQgMzIuNTk0MiA5MS4zMTA4IDM1LjY4NDEgODcuMzcyMyAzOC4zMTRDODMuNDMzOCA0MC44MTIgNzkuNjQzMSA0Mi4zODk2IDc2IDQzLjA0NzRWNzIuNjMwNEM4MC41MjkyIDcxLjQ0NzMgODQuNjE1NCA2OS42NzI0IDg4LjI1ODUgNjcuMzA1N0M5MiA2NC44MDc2IDk1LjI0OTIgNjEuODQ5MSA5OC4wMDYyIDU4LjQzMDdDOTkuNDY1NSA1Ni41NTEzIDEwMC43NDUgNTQuNTk4NiAxMDEuODQ2IDUyLjU3MjNWMTYwSDM1QzE1LjY3IDE2MCAwIDE0NC4zMyAwIDEyNVYzNVoiIGZpbGw9IiNmZmZmZmYiLz48L3N2Zz4K)](https://studio1hq.com)
[![Built with InsForge](https://img.shields.io/badge/Built_with_InsForge-141414?style=for-the-badge)](https://insforge.dev)

[InsForge docs](https://docs.insforge.dev) &nbsp;·&nbsp; [InsForge](https://insforge.dev) &nbsp;·&nbsp; [More examples](https://github.com/Studio1HQ) &nbsp;·&nbsp; [studio1hq.com](https://studio1hq.com)

</div>

<br />

## Overview

A small chat app that shows two InsForge features working together: realtime messaging and vector search. You join a room by name, send messages, and see new messages arrive live over InsForge realtime. A search box finds past messages in the same room by meaning rather than exact words.

Each message is embedded with Google Gemini (`gemini-embedding-001`, 768 dimensions) before it is saved to the InsForge database. A search embeds your query the same way and calls a database function that returns the closest messages in the room.

## Features

- **Rooms**: Join any room by name; the default is `general`. History shows the 50 most recent messages.
- **Realtime updates**: The browser connects with `insforge.realtime`, subscribes to `room:<roomId>` and listens for `NEW_message` events. A status line shows connection state.
- **Embeddings on send**: The `/api/send` route embeds the message text and inserts `room_id`, `content` and `embedding` into `messages`.
- **Semantic search**: The `/api/search` route embeds the query and calls the `match_messages` database function with the query vector and the room.
- **Server-side keys**: Gemini calls run in Next.js route handlers, so the Gemini key never reaches the browser.

## Tech stack

| Layer | Tool |
| --- | --- |
| Backend platform | [InsForge](https://insforge.dev) (database, realtime) via `@insforge/sdk` |
| Embeddings | [Google Gemini](https://ai.google.dev/) `gemini-embedding-001` (`@google/generative-ai`) |
| Framework | [Next.js](https://nextjs.org/) 16 (App Router), React 19, TypeScript |
| Styling | [Tailwind CSS](https://tailwindcss.com/) 4 |

## Getting started

### Prerequisites

- Node.js 20.9 or later (required by Next.js 16)
- An InsForge project, from the [InsForge dashboard](https://insforge.dev)
- A Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey)
- In the InsForge database:
  - a `messages` table with `id`, `room_id`, `content`, `created_at` and a 768-dimension vector column `embedding` (pgvector)
  - a function `match_messages(query_embedding, match_room)` that returns the closest messages in a room
  - a realtime setup that publishes `NEW_message` events to channel `room:<room_id>` when a message is inserted

### Installation

```bash
git clone https://github.com/Studio1HQ/semantic-chat-insforge.git
cd semantic-chat-insforge
npm install
```

### Environment variables

Create `.env.local` in the project root.

| Variable | Description |
| --- | --- |
| `INSFORGE_URL` | InsForge project URL, used by the API routes. |
| `INSFORGE_ANON_KEY` | InsForge anonymous key, used by the API routes. |
| `NEXT_PUBLIC_INSFORGE_URL` | InsForge project URL, used by the browser realtime client. |
| `NEXT_PUBLIC_INSFORGE_ANON_KEY` | InsForge anonymous key, used by the browser realtime client. |
| `GEMINI_API_KEY` | Google Gemini key for embeddings. |

### Run

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). Other scripts: `npm run build`, `npm run start`, `npm run lint`.

## How it works

1. **Join a room**: `page.tsx` loads history from `GET /api/messages/<roomId>`, then connects to realtime and subscribes to `room:<roomId>`.
2. **Send**: `POST /api/send` embeds the text with Gemini and inserts the row into `messages`. Other clients in the room receive a `NEW_message` event.
3. **Search**: `POST /api/search` embeds the query and calls `insforge.database.rpc('match_messages', { query_embedding, match_room })`.

```text
src/app/
├── page.tsx                      # Chat UI, realtime connection, search box
└── api/
    ├── messages/[roomId]/route.ts  # Last 50 messages in a room
    ├── send/route.ts               # Embed and insert a message
    └── search/route.ts             # Embed a query and call match_messages
```

The repo also contains agent skill folders (`.agent`, `.claude` and similar) with InsForge skills for coding assistants. They are not part of the app.

## Resources

- [InsForge docs](https://docs.insforge.dev): database, realtime and the SDK.
- [InsForge](https://insforge.dev): the backend platform used here.
- [Gemini embeddings docs](https://ai.google.dev/gemini-api/docs/embeddings): the embedding model.
- [Next.js documentation](https://nextjs.org/docs): the framework.

<br />

---

<div align="center">

<a href="https://studio1hq.com">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-dark.svg" />
    <img src="https://raw.githubusercontent.com/Studio1HQ/website/main/brand/studio1-logo-on-light.svg" alt="Studio1" width="160" />
  </picture>
</a>

Built by **[Studio1](https://studio1hq.com)**, the technical content and DevRel partner for devtool teams. Made with [InsForge](https://insforge.dev).<br />
Browse more open-source examples at [github.com/Studio1HQ](https://github.com/Studio1HQ).

[![Website](https://img.shields.io/badge/studio1hq.com-0b0807?style=for-the-badge&logo=googlechrome&logoColor=white)](https://studio1hq.com)
[![Book a call](https://img.shields.io/badge/Book_a_strategy_call-f97316?style=for-the-badge&logo=googlecalendar&logoColor=white)](https://cal.com/studio1/collab)
[![X](https://img.shields.io/badge/%40Studio1HQ-141414?style=for-the-badge&logo=x&logoColor=white)](https://x.com/Studio1HQ)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0a66c2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/studio1hq/)

<sub><a href="https://studio1hq.com">studio1hq.com</a> · Technical content, docs, video and DevRel for devtools</sub>

</div>
