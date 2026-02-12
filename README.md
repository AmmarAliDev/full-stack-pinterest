# Full-Stack Pinterest Clone

A full-stack Pinterest-style social app built with a React + TypeScript frontend and an Express + MongoDB backend.

## Tech Stack

- **Frontend:** React 19, TypeScript, Vite 7, TailwindCSS 4, TanStack Query, React Router 7
- **Backend:** Express 5, MongoDB (Mongoose), Socket.IO, JWT (cookie-based auth), ImageKit, Sharp
- **Tooling:** pnpm, ESLint, TypeScript

## Repository Structure

```text
.
├─ client/   # React SPA
└─ backend/  # Express API + Socket.IO server
```

## Key Features

- Authentication (register/login/logout) with HttpOnly cookie token
- Pins feed, pin details, create/update/delete pins, save/like interactions
- Boards, comments, user profiles, follows, and image management
- Real-time conversations and notifications with Socket.IO
- Image optimization/upload helpers with Sharp + ImageKit
- Infinite-scroll style data access in client via TanStack Query

## Prerequisites

- **Node.js:** modern LTS (Node 20+ recommended)
- **Package manager:** `pnpm`
- **Services:** MongoDB instance and ImageKit account

## Quick Start

Install dependencies in each app:

```bash
cd backend
pnpm install

cd ../client
pnpm install
```

Create environment files:

- Backend: `backend/.env` (see [backend/README.md](backend/README.md))
- Client: `client/.env` (see [client/README.md](client/README.md))

Run both apps in separate terminals:

```bash
# terminal 1
cd backend
pnpm dev

# terminal 2
cd client
pnpm dev
```

Defaults:

- API server: `http://localhost:3000`
- Client app: `http://localhost:5173`

## Application Architecture

### Frontend (`client/`)

- API layer in `src/api/` with centralized Axios instance and error normalization
- Server state managed with TanStack Query hooks in `src/hooks/queries` and `src/hooks/mutations`
- Routing and layouts under `src/routes/`
- UI components in `src/components/` (including shadcn/ui based primitives in `src/components/ui`)
- Local client state with Zustand stores in `src/lib/`

### Backend (`backend/`)

- REST route modules under `routes/` and handlers in `controllers/`
- Data models in `models/` via Mongoose
- Auth middleware in `middleware/verifyToken.ts`
- Socket.IO setup in `index.ts` and helper in `utils/socket.ts`
- Database connection in `db/connectDB.ts`

## API Surface (high-level)

- `/auth` - authentication
- `/users` - users and follows
- `/pins` - pins and interactions
- `/boards` - board creation and retrieval
- `/comments` - pin comments
- `/images` - user image assets
- `/conversations` - chat/messaging
- `/notifications` - notification center

See [backend/README.md](backend/README.md) for endpoint-level details.

## Development Notes

- Auth is cookie-based; frontend requests use `withCredentials: true`
- CORS is restricted to `CLIENT_URL` from backend `.env`
- Socket auth accepts token from handshake auth/header and supports cookie fallback
- The project currently has no formal automated test suite configured

## Where to Go Next

- Frontend usage and architecture: [client/README.md](client/README.md)
- Backend setup, env, and routes: [backend/README.md](backend/README.md)