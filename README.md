# PERN Project - Task Manager

A complete full-stack web application built with the **PERN** stack (PostgreSQL, Express, React, Node.js). The project implements a user authentication system based on JWT (stored in `httpOnly` cookies) and a full CRUD (Create, Read, Update, Delete) for user-specific tasks.

---

## 🚀 Main Features

* **User Authentication:** New user registration and login.
* **Session Management:** Uses JSON Web Tokens (JWT) stored in `httpOnly` cookies for a persistent and secure session.
* **Protected Routes:** Backend middleware (`isAuth`) and frontend logic (`ProtectedRoute`) to protect routes.
* **Task CRUD:** Authenticated users can create, view, edit, and delete their own tasks.
* **Relational Database:** Uses PostgreSQL to store users and tasks.
* **User Profile:** Users can view their profile information, including a [Gravatar](https://en.gravatar.com/) generated from their email.

---

## 🛠️ Tech Stack

This project uses a monorepo architecture (or two separate folders) for the `frontend` and `backend`.

### Backend
* **Node.js**
* **Express** (For the server and API routing)
* **PostgreSQL** (Database)
* **`node-postgres` (pg)** (PostgreSQL client for Node.js)
* **`jsonwebtoken` (JWT)** (For generating session tokens)
* **`bcrypt`** (For password hashing)
* **`cookie-parser`** (For handling cookies in Express)
* **`cors`** (To allow communication between frontend and backend)

### Frontend
* **React**
* **Vite** (As a development environment and bundler)
* **React Router** (For client-side navigation and routes)
* **React Hook Form** (For form management and validation)
* **Axios** (For requests to the backend API)
* **Context API** (For global state management for authentication and tasks)
* **Tailwind CSS** (For design and user interface)

---

## 🏁 Getting Started (Installation Guide)

Follow these steps to clone and run the project locally.

### Prerequisites

* **Git**
* **Node.js** (nvm is recommended for managing versions, e.g., `nvm install 20`)
* **PostgreSQL** (Database server)

### 1. Database Setup (PostgreSQL)

First, you need to have PostgreSQL installed and a user and database created for the project.

1.  **Install PostgreSQL** (if you don't already have it):
    ```bash
    sudo apt update
    sudo apt install postgresql postgresql-contrib
    ```

2.  **Access the PostgreSQL console:**
    ```bash
    sudo -u postgres psql
    ```

3.  **Create the user and database** (use the names we defined, or change them and adjust the backend `.env`):
    ```sql
    -- 1. Create the user (role). REMEMBER to change 'your_secure_password'
    CREATE USER usuario WITH SUPERUSER PASSWORD 'your_secure_password';

    -- 2. Create the database
    CREATE DATABASE proyect_pern;

    -- 3. Assign ownership of the database to the user
    ALTER DATABASE proyect_pern OWNER TO usuario;
    ```
4.  (Optional) If you don't have it, install **pgAdmin4** to visualize the database.

### 2. Backend Setup

1.  **Clone the repository:**
    ```bash
    git clone [https://your-repository.git](https://your-repository.git)
    cd your-project/backend 
    ```

2.  **Install the dependencies:**
    ```bash
    npm install
    ```

3.  **Create the environment variables file (`config.js`)** in the root of the `backend` folder and fill it with your database credentials:

    ```env
    # Database Configuration
    export const PORT = process.env.PORT || 3000;
    export const PG_PORT = process.env.PG_PORT || 5432;
    export const PG_HOST = process.env.PG_HOST || "localhost";
    export const PG_USER = process.env.PG_USER || "";
    export const PG_PASSWORD = process.env.PG_PASSWORD || "";
    export const PG_DATABASE = process.env.PG_DATABASE || "";
    export const ORIGIN = process.env.ORIGIN || "http://localhost:5173";

    # Secret to sign the JWTs (use a long, random string)
    TOKEN_SECRET=un_secreto_muy_dificil_de_adivinar_123

    # Frontend URL (for CORS)
    FRONTEND_URL=http://localhost:5173
    ```

4.  **Run the backend server:**
    ```bash
    npm run dev
    ```
    (The server should be running on `http://localhost:3000`)

### 3. Frontend Setup

1.  **Open a new terminal** and navigate to the frontend folder:
    ```bash
    cd your-project/frontend
    ```

2.  **Install the dependencies:**
    ```bash
    npm install
    ```

3.  (Optional but recommended) Create a `.env` file in the `frontend` root to define the API URL:
    ```env
    VITE_API_URL=http://localhost:3000/api
    ```
    *Make sure your `axios.js` file reads this variable (`import.meta.env.VITE_API_URL`) as its `baseURL`.*

4.  **Run the Vite development client:**
    ```bash
    npm run dev
    ```
    (The application will be available at `http://localhost:5173`)

---

## 📝 Note on Cookies in Development (httpOnly)

This project is configured to run in an `http://localhost` development environment.

The cookie configuration in the backend (`auth.controller.js`) is defined as:
```javascript
res.cookie("token", token, {
    httpOnly: true,
    sameSite: "lax",
    secure: false // <-- false is crucial for localhost
});