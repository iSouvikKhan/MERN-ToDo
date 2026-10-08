# MERN ToDo

A full-stack to-do list application built with MongoDB, Express, React and Node.js. Users register and log in, then create, edit, complete and delete their own to-do items. Authentication uses a JWT stored in an HTTP-only cookie.

## Features

- User registration and login with server-side validation (email format, password length, matching confirm password)
- Passwords hashed with bcryptjs; JWT (7-day expiry) issued in an HTTP-only `access-token` cookie
- Protected API routes: each user can only see and modify their own to-dos
- Create, edit, delete to-dos (content limited to 300 characters)
- Mark to-dos as complete or incomplete; completed items are shown in a separate list
- Express serves the production React build, so the app can run as a single server

## Tech Stack

- **Backend:** Node.js, Express, Mongoose (MongoDB), jsonwebtoken, bcryptjs, cookie-parser, validator, dotenv
- **Frontend:** React 17 (Create React App), React Router v6, Context API with `useReducer`, Axios, Sass
- **Dev tools:** nodemon, concurrently

## Project Structure

```
MERN-ToDo/
├── server.js              # Express app, MongoDB connection, static client build
├── routes/
│   ├── auth.js            # /api/auth routes (register, login, current, logout)
│   └── todos.js           # /api/todos routes (CRUD, complete/incomplete)
├── models/
│   ├── User.js            # User schema (email, password, name)
│   └── ToDo.js            # ToDo schema (user, content, complete, completedAt)
├── middleware/
│   └── permissions.js     # requiresAuth: verifies the JWT cookie
├── validation/            # Input validation helpers
└── client/                # React frontend
    └── src/
        ├── components/    # Header, AuthBox, Dashboard, NewToDo, ToDoCard, Layout
        ├── context/       # GlobalContext (user and to-do state)
        └── main.scss
```

## Prerequisites

- Node.js and npm
- A MongoDB database (local or hosted, e.g. MongoDB Atlas)

## Installation

```bash
git clone https://github.com/iSouvikKhan/MERN-ToDo.git
cd MERN-ToDo
npm install
npm run install-client
```

### Environment variables

Create a `.env` file in the project root. The server reads these variables:

| Variable     | Purpose                                                                 |
|--------------|-------------------------------------------------------------------------|
| `MONGO_URI`  | MongoDB connection string                                               |
| `PORT`       | Port the Express server listens on (use `5000` in development, since the React dev server proxies API requests to `http://localhost:5000`) |
| `JWT_SECRET` | Secret used to sign and verify JWTs                                     |
| `NODE_ENV`   | Optional; when set to `production`, the auth cookie is marked `secure`  |

## Running

Development (runs the React dev server and the Express server with nodemon together):

```bash
npm run dev
```

The React app opens on the Create React App dev server (port 3000 by default) and forwards `/api` requests to the Express server.

Other scripts:

| Command                  | Description                                  |
|--------------------------|----------------------------------------------|
| `npm run server`         | Start only the Express server with nodemon   |
| `npm run client`         | Start only the React dev server              |
| `npm run build`          | Build the React app into `client/build`      |
| `npm start`              | Start the Express server with Node           |

Production-style run (Express serves the built React app):

```bash
npm run build
npm start
```

Then open `http://localhost:<PORT>`.

## API Endpoints

| Method | Endpoint                         | Auth | Description                     |
|--------|----------------------------------|------|---------------------------------|
| POST   | `/api/auth/register`             | No   | Register a new user             |
| POST   | `/api/auth/login`                | No   | Log in and set the auth cookie  |
| GET    | `/api/auth/current`              | Yes  | Get the logged-in user          |
| PUT    | `/api/auth/logout`               | Yes  | Clear the auth cookie           |
| POST   | `/api/todos/new`                 | Yes  | Create a to-do                  |
| GET    | `/api/todos/current`             | Yes  | Get the user's complete and incomplete to-dos |
| PUT    | `/api/todos/:toDoId`             | Yes  | Update a to-do's content        |
| PUT    | `/api/todos/:toDoId/complete`    | Yes  | Mark a to-do as complete        |
| PUT    | `/api/todos/:toDoId/incomplete`  | Yes  | Mark a to-do as incomplete      |
| DELETE | `/api/todos/:toDoId`             | Yes  | Delete a to-do                  |
