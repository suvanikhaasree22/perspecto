# PERSPECTO
### Every Story. Every Perspective.

PERSPECTO is a calm, editorial full-stack blogging and social discussion platform built around thoughtful writing, perspectives, collaborative ideas, community fact-checking, mood expression, and future-locked time capsules.

## Product highlights
- JWT authentication with bcrypt password hashing
- Blog, Discussion, Idea and Time Capsule posts
- Likes, bookmarks, comments, threaded replies model, perspectives and follows
- Trending/search APIs backed by MongoDB
- FactLens claims + evidence
- IdeaForge collaborative contributions
- Time Capsules with server-side unlock protection
- Community mood UI
- Notifications
- Responsive desktop/mobile dashboard
- Light/dark mode
- Vercel + Render + MongoDB Atlas ready

## Tech stack
**Frontend:** HTML5, CSS3, vanilla JavaScript, responsive SPA-style UI

**Backend:** Node.js, Express.js, Mongoose, JWT, bcryptjs, CORS, rate limiting

**Database:** MongoDB / MongoDB Atlas

## Project structure
```text
Perspecto/
├── frontend/
│   ├── index.html
│   ├── css/style.css
│   ├── js/app.js
│   ├── pages/
│   └── assets/
├── backend/
│   ├── config/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   ├── server.js
│   ├── package.json
│   └── .env.example
├── .env.example
├── .gitignore
└── README.md
```

## 1. MongoDB Atlas setup
1. Create a MongoDB Atlas project and cluster.
2. Create a database user.
3. Add your development IP to Network Access.
4. Copy the Node.js connection string.
5. Use a database name such as `perspecto`.

## 2. Backend setup
```bash
cd backend
npm install
copy .env.example .env
```

Edit `.env`:
```env
PORT=5000
MONGODB_URI=your_mongodb_atlas_connection_string
JWT_SECRET=use_a_long_random_secret
CLIENT_URL=http://localhost:5500
```

Run development server:
```bash
npm run dev
```

Or:
```bash
npm start
```

Health check:
`http://localhost:5000/api/health`

## 3. Frontend setup
The frontend is static and can be served using VS Code Live Server or any static HTTP server.

Recommended:
1. Open `frontend/` in VS Code.
2. Start Live Server.
3. Open the shown URL, normally `http://localhost:5500`.

The frontend defaults to:
```text
http://localhost:5000/api
```

To change the API URL without editing source code, open the browser console once and run:
```js
localStorage.setItem('perspecto_api', 'https://YOUR-BACKEND.onrender.com/api')
location.reload()
```

## 4. GitHub
Create/use:
```text
https://github.com/suvanikhaasree22/PERSPECTO
```

Never commit `.env`. The repository includes `.env.example` instead.

```bash
git init
git add .
git commit -m "Build PERSPECTO full-stack platform"
git branch -M main
git remote add origin https://github.com/suvanikhaasree22/PERSPECTO.git
git push -u origin main
```

## 5. Render deployment — backend
1. Create a new Web Service on Render.
2. Connect the GitHub repository.
3. Set **Root Directory** to `backend`.
4. Build command: `npm install`
5. Start command: `npm start`
6. Add environment variables:
```env
MONGODB_URI=...
JWT_SECRET=...
CLIENT_URL=https://YOUR-VERCEL-DOMAIN.vercel.app
PORT=10000
```
Render provides the public backend URL.

## 6. Vercel deployment — frontend
1. Import the same GitHub repository into Vercel.
2. Set Root Directory to `frontend`.
3. Deploy as a static site.
4. After the Render backend is live, set the browser API configuration to the Render URL using `localStorage` as described above, or replace the `API` constant in `frontend/js/app.js` before deployment.

## 7. API overview
### Auth
- `POST /api/auth/register`
- `POST /api/auth/login`

### Posts
- `GET /api/posts`
- `POST /api/posts`
- `GET /api/posts/:id`
- `PUT /api/posts/:id`
- `DELETE /api/posts/:id`
- `POST /api/posts/:id/like`
- `POST /api/posts/:id/bookmark`
- `POST /api/posts/:id/perspective`
- `GET /api/posts/:id/comments`
- `POST /api/posts/:id/comments`

### Ideas
- `GET /api/ideas`
- `POST /api/ideas`
- `POST /api/ideas/:id/contributions`
- `GET /api/ideas/:id/contributions`

### FactLens
- `GET /api/factlens/post/:postId`
- `POST /api/factlens`
- `POST /api/factlens/:id/evidence`

### Time Capsules
- `GET /api/capsules/mine`
- `POST /api/capsules`
- `GET /api/capsules/:id`

### Users
- `GET /api/users/me`
- `PUT /api/users/me`
- `GET /api/users/search`
- `POST /api/users/:id/follow`
- `GET /api/users/:id`
- `GET /api/users/me/notifications`
- `PATCH /api/users/me/notifications/read`

### Search
- `GET /api/search?q=...`

## Security notes
- Passwords are hashed using bcryptjs.
- Authentication uses signed JWTs.
- Protected routes require `Authorization: Bearer <token>`.
- Secrets are loaded through environment variables.
- `.env` is ignored by Git.
- Express rate limiting is enabled.
- Time capsule content remains server-side locked until its unlock date.

## Troubleshooting
**MONGODB_URI is not set**
- Copy `backend/.env.example` to `backend/.env` and add the Atlas URI.

**MongoDB connection refused**
- Check Atlas Network Access and database credentials.

**CORS issue**
- Confirm the frontend origin is allowed by your deployed backend and set `CLIENT_URL` appropriately.

**Frontend shows demo stories**
- The UI intentionally has a graceful demo fallback when the backend is unavailable. Once the backend is running and MongoDB is configured, registration/login and database-backed features become active.

**PowerShell blocks npm/npx scripts**
- Use Command Prompt, or update the execution policy for your own Windows user if permitted by your machine's security policy.

## Viva explanation
PERSPECTO separates presentation from data services. The frontend communicates with REST APIs. Express routes delegate database work to Mongoose models. JWT middleware protects write operations. MongoDB stores users, posts, interactions, discussions, ideas, fact claims, evidence, notifications and time capsules. This keeps the project understandable while still following a realistic startup architecture.
