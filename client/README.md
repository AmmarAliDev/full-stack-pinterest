# Client (Frontend)

React 19 + TypeScript single-page app for the Pinterest clone UI.

## Stack

- React 19 + TypeScript + Vite 7
- TailwindCSS 4
- TanStack Query for server state
- React Router 7
- Axios for HTTP requests
- Socket.IO client for real-time chat/notifications
- Zustand for local app state

## Setup

```bash
pnpm install
```

Create `.env` in `client/`:

```env
VITE_API_BASE_URL=http://localhost:3000
VITE_IK_IMAGEKIT_URL=https://ik.imagekit.io/your_imagekit_id
VITE_API_IK_URL_ENDPOINT=https://ik.imagekit.io/your_imagekit_id
VITE_GIPHY_SDK_KEY=your_giphy_api_key
```

## Scripts

- `pnpm dev` - start Vite development server
- `pnpm build` - type-check and production build
- `pnpm preview` - preview built app
- `pnpm start` - alias to `vite preview`
- `pnpm lint` - run ESLint

## Frontend Architecture

```text
src/
├─ api/            # axios instance + endpoint modules
├─ components/     # reusable and feature components
├─ hooks/          # custom hooks + query/mutation hooks
├─ lib/            # local stores/utilities
├─ routes/         # route pages + layouts
├─ types/          # domain + api + store type definitions
└─ assets/
```

### Routing

Main routes are declared in `src/main.tsx`:

- `/` - home feed
- `/create` - pin creation
- `/pin/:id` - pin detail page
- `/user/:id` - profile page
- `/user/edit/:id` - profile editing
- `/auth` - login/register

### Data Layer

- Central Axios client in `src/api/axios.ts`
- `withCredentials: true` for cookie auth
- Adds `X-Request-ID` header per request
- Response interceptor unwraps `.data`
- API modules grouped by resource in `src/api/endpoints/`

### State Management

- **Server state:** TanStack Query hooks
- **Client state:** Zustand stores (`authStore`, `socketStore`, `drawerStore`, `editorStore`)

### Real-Time

- Socket connection established in `src/routes/layouts/MainLayout.tsx`
- Uses backend URL from `VITE_API_BASE_URL`
- Supports conversation updates and notifications

## UI and Media

- TailwindCSS utility styling
- shadcn/ui style components in `src/components/ui/`
- Image rendering through ImageKit via `src/components/image/Image.tsx`
- Sticker picker integration via GIPHY SDK key

## Development Notes

- Uses `@/` alias for `src/`
- React Compiler Babel plugin is enabled
- Auth user state is persisted in browser storage via Zustand middleware
- Backend must allow frontend origin with credentials (`CLIENT_URL` on backend)

## Related Docs

- Project overview: [`../README.md`](../README.md)
- API/server details: [`../backend/README.md`](../backend/README.md)
