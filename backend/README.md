# Backend (API Server)

Express 5 + TypeScript backend for the Pinterest clone, including REST APIs, auth, image handling, and real-time events.

## Stack

- Express 5
- MongoDB + Mongoose
- JWT authentication (HttpOnly cookie token)
- Socket.IO for real-time messaging/notifications
- ImageKit SDK + Sharp for media processing

## Setup

```bash
pnpm install
```

Create `.env` in `backend/`:

```env
PORT=3000
CLIENT_URL=http://localhost:5173
MONGO_DB_URI=mongodb+srv://<user>:<pass>@<cluster>/<db>
JWT_SECRET=replace-with-secure-secret
NODE_ENV=development

# ImageKit
IK_URL_ENDPOINT=https://ik.imagekit.io/your_imagekit_id
IK_PRIVATE_KEY=your_imagekit_private_key

# Optional image processing limits
MAX_IMAGE_BYTES=10485760
MAX_IMAGE_DIMENSION=2048
```

## Run

```bash
pnpm dev
```

The `dev` script runs:

```bash
node --watch --env-file=.env index.ts
```

Default URL: `http://localhost:3000`

## API Route Map

Base route modules registered in `index.ts`:

### Auth (`/auth`)

- `POST /register`
- `POST /login`
- `POST /logout`

### Users (`/users`)

- `GET /:id`
- `GET /`
- `PATCH /` (auth required)
- `POST /follow/:username` (auth required)
- `POST /delete/:id`

### Pins (`/pins`)

- `GET /`
- `POST /` (auth required)
- `PATCH /:id` (auth required)
- `GET /:id`
- `DELETE /:id` (auth required)
- `POST /interact/:id` (auth required)
- `GET /interaction-check/:id`
- `GET /saved-pins/:userId`

### Boards (`/boards`)

- `GET /users` (auth required)
- `POST /`

### Comments (`/comments`)

- `GET /`
- `GET /:pinId`
- `POST /` (auth required)
- `PATCH /:commentId` (auth required)
- `DELETE /:commentId` (auth required)

### Images (`/images`)

- `GET /` (auth required)
- `POST /` (auth required)
- `PATCH /:id` (auth required)
- `DELETE /:id` (auth required)

### Conversations (`/conversations`)

- `POST /` (auth required)
- `GET /:conversationId` (auth required)
- `GET /participants/:userId` (auth required)
- `GET /` (auth required)
- `PUT /mark-read/:conversationId` (auth required)

### Notifications (`/notifications`)

- `GET /` (auth required)
- `POST /` (auth required)
- `PATCH /mark-read` (auth required)
- `DELETE /:notificationId` (auth required)

## Authentication Model

- Login/register issue a signed JWT and store it in an HttpOnly cookie named `token`
- Protected endpoints use `middleware/verifyToken.ts`
- Verification reads `req.cookies.token`
- When token is missing/invalid, API returns `401` or `403`

## CORS and Cookies

- CORS allows only `CLIENT_URL` (plus non-browser/no-origin requests)
- `credentials: true` is enabled
- Allowed headers include `Content-Type`, `Authorization`, and `X-Request-ID`
- Frontend must call APIs with credentials enabled

## Socket.IO

- Socket server is attached to the same HTTP server in `index.ts`
- Auth expects token via:
  - `handshake.auth.token`, or
  - `Authorization` header bearer token, or
  - `token` cookie fallback
- Connected user joins a room named by `userId`
- `utils/socket.ts` exposes `setIO/getIO` for emitting from controllers/services

## Backend Architecture

```text
backend/
├─ controllers/   # route handlers/business logic
├─ routes/        # API route definitions
├─ models/        # Mongoose schemas
├─ middleware/    # auth middleware
├─ db/            # DB connection
├─ services/      # domain services (e.g., notifications)
└─ utils/         # image + socket + seed helpers
```

## Development Notes

- Ensure MongoDB is reachable before starting server
- Secure cookie behavior depends on `NODE_ENV` (`secure` in production)
- Keep `JWT_SECRET` and `IK_PRIVATE_KEY` out of source control
- No formal automated tests are currently configured

## Related Docs

- Project overview: [`../README.md`](../README.md)
- Frontend guide: [`../client/README.md`](../client/README.md)