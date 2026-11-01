# LearnHub — Setup Guide

LearnHub is a full-stack learning platform with a React + Vite frontend and an Express + MongoDB backend. Students can browse and enroll in courses, stream lessons, complete them, take quizzes and track progress. Instructors can create courses, upload videos, build quizzes and review student results.

This folder contains:

- `learning-hub-backend/` — Express REST API, MongoDB models, demo seed script
- `learning-hub-frontend/` — React + Vite single-page app

## Requirements

- Node.js 20 or newer (`node --version`)
- A MongoDB database. Free MongoDB Atlas works: https://www.mongodb.com/atlas
- Optional: a code editor such as VS Code

## 1. Start the backend

Open a terminal in `learning-hub-backend`.

```powershell
npm install
Copy-Item .env.example .env
```

Open `.env` and set:

```
MONGODB_URI=mongodb+srv://USERNAME:PASSWORD@CLUSTER.mongodb.net/learning_hub?retryWrites=true&w=majority
JWT_SECRET=<at least 32 random characters>
CLIENT_ORIGIN=http://localhost:5173
```

Generate a secret with:

```powershell
node -e "console.log(require('crypto').randomBytes(48).toString('hex'))"
```

Create demo courses, lessons and accounts (safe to run more than once):

```powershell
npm run seed
```

Start the API:

```powershell
npm run dev
```

The API runs at `http://localhost:5000`. Check it with `http://localhost:5000/api/health`.

## 2. Start the frontend

Open a second terminal in `learning-hub-frontend`.

```powershell
npm install
npm run dev
```

Open `http://localhost:5173`. The frontend defaults to `http://localhost:5000/api`, so no `.env` file is needed locally. To use a different API URL, copy `.env.example` to `.env.local` and change `VITE_API_URL`.

## Demo accounts

Created by `npm run seed`:

| Role | Email | Password |
|---|---|---|
| Student | `student@learnhub.dev` | `LearnPass123!` |
| Instructor | `instructor@learnhub.dev` | `TeachPass123!` |

The login page also has demo buttons that fill these credentials while running in development.

## Features

Students:

- Register and sign in with real JWT authentication
- Explore and search published courses
- Preview free lessons before enrolling
- Enroll and stream protected lesson videos
- Mark lessons complete and track progress
- Take server-scored quizzes with optional time limits
- Review lesson activity, quiz attempts and scores

Instructors:

- Dashboard with courses, students, lessons and quiz statistics
- Create, publish, unpublish and delete courses
- Add lessons and upload MP4/WebM/MOV/MKV videos
- Mark lessons as free previews
- Build and edit quizzes with a visual question editor
- Review enrolled students, progress and quiz results
- Export students and quiz results as CSV

## Production build

Frontend:

```powershell
cd learning-hub-frontend
npm run build
```

Backend:

```powershell
cd learning-hub-backend
npm start
```

Both folders include a `README.md` with the full API route map and deployment notes for Vercel.

## Security notes

- `.env` files are **not** included in this package. Create your own from `.env.example`.
- Never commit `.env`, database passwords or JWT secrets.
- Uploaded videos are stored in `learning-hub-backend/uploads/videos`. On serverless hosts such as Vercel the filesystem is temporary, so use external object storage for production video hosting.
- Set `CLIENT_ORIGIN` to your deployed frontend URL in production and use HTTPS.
