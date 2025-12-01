# News Explorer Backend

Node.js/Express API for the News Explorer app. It provides user registration/login with JWT auth, MongoDB persistence for saved articles, rate limiting, security headers, and request/error logging.

## Stack

- Node.js 18+, Express, MongoDB/Mongoose
- Auth with JWT (bearer token via `Authorization` or `x-auth-token`)
- Validation with `express-validator`
- Security middleware: `helmet`, `express-rate-limit`, `cors`
- Logging with `winston`/`express-winston`

## Getting Started

1. Install dependencies: `npm install`
2. Set environment variables (see `.env.example` below).
3. Start the API:
   - Dev: `npm run dev` (nodemon)
   - Prod: `npm start`

The server listens on `PORT` (defaults to `3000`).

### Environment Variables

Add a `.env` file at the project root:

```
PORT=3000
CONNECTION=mongodb://localhost:27017/myLocalDB
JWT_SECRET=your-jwt-secret
```

## API

All routes are prefixed at the root (e.g., `http://localhost:3000/signup`).

- `POST /signup` — create user. Body: `{ "name", "email", "password" }`
- `POST /signin` — login. Body: `{ "email", "password" }` → returns `{ token }`
- `GET /users/me` — current user profile. Headers: `Authorization: Bearer <token>`
- `GET /articles` — list saved articles for the authenticated user.
- `POST /articles` — create article. Auth required. Body: `{ "keyword", "title", "text", "date", "source", "link", "image" }`
- `DELETE /articles/:articleId` — remove an article by id (owner only). Auth required.

## Project Structure

- `app.js` — app entrypoint and middleware setup
- `routes/` — route definitions
- `controllers/` — request handlers
- `models/` — Mongoose schemas
- `middleware/` — auth, rate limit, logging, and error helpers
- `utils/` — reusable constants
- `logs/` — request/error logs (gitignored)

## Scripts

- `npm start` — run server
- `npm run dev` — run with nodemon
- `npm run lint` — lint with ESLint

## Notes

- The code uses bearer tokens; provide `Authorization: Bearer <token>` (or `x-auth-token`) on protected routes.
- Logs are written to `logs/request.log` and `logs/error.log` (ensure the `logs` folder exists).
