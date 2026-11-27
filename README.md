# Nexboard — Collaborative Kanban Board

<p align="center">
  <a href="https://nexboard-hoichoi-surajit.vercel.app/">
    <img src="https://img.shields.io/badge/Live_Demo-Visit_Nexboard-red?style=for-the-badge" alt="Live Demo" />
  </a>
</p>

**Live Demo:** https://nexboard-hoichoi-surajit.vercel.app/

Nexboard is a collaborative Kanban board application built with a **Next.js + TypeScript frontend** and a modular **Node.js + Express + MongoDB backend**. It helps users organize, manage, and track tasks through an intuitive drag-and-drop interface with real-time collaboration.

---

## 🚀 Project Structure

The project is organized into two main directories:

- `nexboard/` — Existing Next.js frontend with TypeScript.
- `backend/` — Modular Node.js, Express, TypeScript, and MongoDB backend.

### Backend Structure

```text
backend/
├── src/
│   ├── config/
│   │   ├── env.ts
│   │   └── db.ts
│   ├── models/
│   │   ├── Task.ts
│   │   ├── Activity.ts
│   │   └── Profile.ts
│   ├── services/
│   │   ├── board.service.ts
│   │   ├── events.service.ts
│   │   └── seed.service.ts
│   ├── routes/
│   │   ├── board.routes.ts
│   │   ├── tasks.routes.ts
│   │   ├── events.routes.ts
│   │   └── profile.routes.ts
│   └── middleware/
│       └── error.ts
```

---

## ✨ Features

- **Collaborative Kanban Board:** Organize and manage tasks across different columns.
- **Drag and Drop:** Move and reorder tasks between columns.
- **Real-Time Collaboration:** Server-Sent Events (SSE) synchronize updates between connected clients.
- **Task Management:** Create, edit, update, move, and delete tasks.
- **Activity Tracking:** Maintain a record of recent board activities.
- **Optimistic UI Updates:** Update the interface immediately without waiting for the server.
- **Automatic Rollback:** Restore failed optimistic changes when backend requests fail.
- **Conflict Protection:** Version-based synchronization prevents stale updates from overwriting newer changes.
- **URL State Persistence:** Preserve board query state through URL parameters.
- **Profile Management:** Update user avatar images.
- **Initial Board Seeding:** Automatically populate the board with initial data when required.
- **MongoDB Integration:** Persist application data using MongoDB and Mongoose.
- **Request Validation:** Validate API requests using Zod.

---

## 🛠️ Technology Stack

### Frontend
- Next.js
- React
- TypeScript
- Tailwind CSS
- Server communication through REST APIs
- Server-Sent Events (SSE)

### Backend
- Node.js
- Express.js
- TypeScript
- MongoDB
- Mongoose
- Zod

### Development Tools
- pnpm
- Git
- GitHub
- Vercel

---

## 📋 Prerequisites

Before running the project, make sure you have installed:

- Node.js 20 or higher
- pnpm
- MongoDB Atlas account or a local MongoDB instance
- Git

---

## ⚙️ Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Surajitjhalda2003/nexboard.git
cd nexboard
```

### 2. Backend Setup

Navigate to the backend directory:

```bash
cd backend
```

Install dependencies:

```bash
pnpm install
```

Create the environment file:

```bash
cp .env.example .env
```

Configure your `backend/.env` file:

```env
PORT=4000
MONGODB_URI=your_mongodb_connection_string
```

Make sure your MongoDB Atlas IP address is whitelisted under **Network Access**.

Start the backend development server:

```bash
pnpm dev
```

The backend will run at:

http://localhost:4000

### 3. Frontend Setup

Open another terminal and navigate to the frontend directory:

```bash
cd nexboard
```

Install dependencies:

```bash
pnpm install
```

Create the `.env.local` file:

```env
NEXT_PUBLIC_API_URL=http://localhost:4000
```

Start the frontend development server:

```bash
pnpm dev
```

The frontend will run at:

http://localhost:3000

---

## 🌐 Live Deployment

**Access the deployed application here:**

### [🚀 Launch Nexboard](https://nexboard-hoichoi-surajit.vercel.app/)

---

## 🔗 API Endpoints

The backend provides the following REST API endpoints:

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | Check server and MongoDB connection health |
| GET | `/api/board` | Fetch tasks, recent activity, and profile images |
| POST | `/api/tasks` | Create a new task |
| PATCH | `/api/tasks/:id` | Update task details or move/reorder tasks |
| DELETE | `/api/tasks/:id` | Delete a task and normalize positions |
| GET | `/api/events` | Subscribe to real-time SSE events |
| PUT | `/api/profile` | Update user avatar image |

---

## ⚡ Optimistic Updates and Rollback

Nexboard uses optimistic UI updates to provide a fast and responsive user experience.

When a user creates, edits, deletes, or moves a task:

1. The React state updates immediately.
2. An API request is sent to the backend.
3. If the request succeeds, the server data becomes authoritative.
4. If the request fails, the frontend rolls back only the affected optimistic changes.
5. A toast notification informs the user about the failure.

Each optimistic update uses an incremented `version` number to avoid reverting newer changes accidentally.

Failure simulation can be enabled using:

```env
SIMULATE_FAILURE_RATE=0.2
```

Alternatively, requests can include:

```http
x-simulate-failure: true
```

---

## 🔄 Real-Time Collaboration

Nexboard uses **Server-Sent Events (SSE)** to broadcast board changes to connected clients.

Supported events include:

- `TASK_CREATED`
- `TASK_UPDATED`
- `TASK_MOVED`
- `TASK_DELETED`
- `ACTIVITY`

### Conflict Prevention

Each task maintains a monotonically increasing `version` number.

Incoming remote events are processed using the following rules:

- Events are deduplicated using `event.id`.
- Remote updates overwrite local tasks only when `remote.version >= local.version`.
- Deleted task IDs are tracked using `remoteDeletedIds`.
- Failed local deletes do not resurrect tasks that have already been deleted by another collaborator.

This approach helps prevent stale remote events from overwriting newer local changes.

---

## 🗄️ Database

Nexboard uses MongoDB with Mongoose for persistent data storage.

### Models

| Model | Description |
|---|---|
| Task | Stores task details, column information, positions, and versions |
| Activity | Stores recent board activity |
| Profile | Stores user profile and avatar information |

Initial board data is automatically seeded when required.

---

## 🔐 MongoDB Atlas Configuration

To connect the backend to MongoDB Atlas:

1. Create a MongoDB Atlas cluster.
2. Create a database user.
3. Copy your MongoDB connection string.
4. Add the connection string to `backend/.env`.
5. Open **Network Access** in MongoDB Atlas.
6. Whitelist your current IP address.

For development only, you can use `0.0.0.0/0` to allow connections from any IP address. Avoid this unrestricted setting in production.

---

## 📦 Deployment

### Frontend Deployment

The frontend can be deployed using Vercel.

1. Import the GitHub repository into Vercel.
2. Configure the frontend project root as `nexboard/`.
3. Set the environment variable:

```env
NEXT_PUBLIC_API_URL=https://your-backend-url.com
```

4. Deploy the application.

### Backend Deployment

The backend can be deployed using a Node.js-compatible hosting provider.

1. Configure the backend project root as `backend/`.
2. Install dependencies using `pnpm install`.
3. Configure the production environment variables.
4. Set the MongoDB Atlas connection string.
5. Build and start the backend using the appropriate production scripts from `package.json`.

Make sure the backend allows requests from your deployed frontend origin and supports long-lived SSE connections.

---

## 🤖 Coding Assistance

- **Antigravity (Google DeepMind)**
- **v0 AI** — Used for frontend development assistance.

No external coding agent or service credentials are required to run the application.

---

## 👨‍💻 Author

**Surajit Das**

- GitHub: [@Surajitjhalda2003](https://github.com/Surajitjhalda2003)
- Project Repository: [Nexboard](https://github.com/Surajitjhalda2003/nexboard)
- Live Application: [https://nexboard-hoichoi-surajit.vercel.app/](https://nexboard-hoichoi-surajit.vercel.app/)

---

## 📄 License

This project is available for educational and development purposes. Add a specific open-source license if you intend to distribute it under particular licensing terms.

---

<p align="center">
  **Built with ❤️ using Next.js, TypeScript, Node.js, Express, and MongoDB.**
</p>
