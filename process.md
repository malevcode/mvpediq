# ComediQ Technical Build Process

This document explains how each code component for ComediQ is built and how they work together to create our functional web app's frontend and backend structure.

## 1. Overall Architecture

ComediQ will follow a **Client-Server Architecture**.
* **Frontend (Client):** Built with React, HTML, CSS, and plain JavaScript, running in the user's web browser. This is responsible for rendering the User Interface (UI) and handling user interactions.
* **Backend (Server):** Built with Node.js and Express.js, running on a server. This is responsible for:
    * Storing and retrieving data from our database (SQLite).
    * Handling user authentication (in later stages).
    * Serving data (API responses) to the frontend.
    * Potentially managing complex business logic.

The frontend communicates with the backend using standard HTTP requests (e.g., GET for fetching data, POST for sending data).

## 2. Frontend Development Process (React, HTML, CSS, JavaScript)

### 2.1 Setting Up the React Application
1.  **Tool:** We use `Create React App` (CRA) to bootstrap our React project. This command-line tool sets up a complete React development environment, including a build pipeline, without requiring manual configuration of tools like Webpack or Babel.
    * **Command:** `npx create-react-app comediq-frontend`
    * **Purpose:** This creates a `comediq-frontend` directory with all necessary files for a React app: `public` (for `index.html`), `src` (for JavaScript components, CSS), `node_modules`, and configuration files.
2.  **Core Files:**
    * `public/index.html`: The single HTML file that serves as the entry point for our React app. React injects our application components into a specific `div` element within this file.
    * `src/index.js`: The main JavaScript file that renders our root React component (`App.js`) into the `index.html`.
    * `src/App.js`: Our primary React component, which will contain the main layout and routing for our application.
    * `src/index.css` / `src/App.css`: Global and component-specific CSS files for styling.
3.  **Development Workflow:**
    * We write React components as JavaScript functions or classes.
    * These components use JSX (JavaScript XML) which allows us to write HTML-like syntax directly within our JavaScript code. CRA's build process transpiles this into regular JavaScript.
    * Styling is done using standard CSS files imported into our components.

### 2.2 UI Layout and Components
* **Bottom Tab Navigation:** This will be a core React component that renders the 5 navigation buttons. Each button will likely be a `div` or `button` element styled with CSS, and their `onClick` handlers will update the application's state to switch between different "pages" or views.
* **Pages (e.g., Home, Find Mics):** Each "page" will be a distinct React component.
    * For example, `FindMicsPage.js` will contain the structure for the two tabs (Map View, List View) and handle their internal state.
    * List view will contain sub-components for individual mic listings.
* **Data Flow (Frontend):** React components manage their own internal `state` (data specific to that component) and receive `props` (data passed down from parent components). When state changes, React efficiently re-renders only the necessary parts of the UI.

## 3. Backend Development Process (Node.js, Express.js, SQLite3)

### 3.1 Setting Up the Node.js/Express.js Server
1.  **Initialization:**
    * Create a new directory for the backend: `mkdir comediq-backend`.
    * Initialize a Node.js project: `cd comediq-backend && npm init -y`. This creates a `package.json` file.
    * Install Express: `npm install express`.
2.  **Server Structure:**
    * `server.js` (or `app.js`): This will be the main entry point for our backend server.
    * It will `require('express')` to create an Express application instance.
    * It will define API routes (e.g., `/api/mics`, `/api/users`) using `app.get()`, `app.post()`, etc.
    * It will start the server listening on a specific port.
3.  **CORS (Cross-Origin Resource Sharing):** Since our frontend (e.g., `localhost:3000` during development) will run on a different "origin" than our backend (e.g., `localhost:5000`), we will need to enable CORS on our Express server to allow requests from the frontend. This will involve adding a minimal Express middleware.

### 3.2 Database Integration (SQLite3)
1.  **Installation:** `npm install sqlite3`. This installs the `node-sqlite3` driver.
2.  **Database File:** SQLite databases are simply files (e.g., `comediq.db`). We will create this file programmatically or have it generated on first run.
3.  **Interaction:**
    * In our Express routes, we will use the `sqlite3` library to open a connection to our database file.
    * We will execute SQL queries (e.g., `CREATE TABLE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE`) to manage our data.
    * For example, an API endpoint like `/api/mics` might query the `mics` table in SQLite and return the data as JSON to the frontend.

### 3.3 Backend API Endpoints (Example)
* **GET /api/mics:** Retrieves a list of open mics from the SQLite database.
* **POST /api/mics:** Adds a new open mic to the SQLite database (used by the "List a Mic" feature).
* **GET /api/users/:id:** Retrieves a user's profile or data.

## 4. How Frontend and Backend Work Together

1.  **User Action:** A user clicks a button or navigates to a page on the ComediQ frontend.
2.  **Frontend Request:** The React component, using standard JavaScript's `fetch` API (or a simple wrapper we create for convenience), sends an HTTP request to a specific endpoint on the backend server.
    * Example: When the "Find Mics" page loads, it might make a `GET` request to `http://localhost:5000/api/mics`.
3.  **Backend Processing:**
    * The Express server receives the request.
    * The corresponding route handler (e.g., `app.get('/api/mics', ...)`) is executed.
    * This handler interacts with the SQLite database (e.g., `db.all('SELECT * FROM mics', ...)`) to fetch the requested data.
    * The data is prepared (e.g., converted to JSON format).
4.  **Backend Response:** The Express server sends an HTTP response back to the frontend, typically containing the requested data in JSON format.
5.  **Frontend Renders:** The React component receives the JSON data. It updates its internal state, which triggers a re-render of the UI, displaying the fetched data to the user.

## 5. Deployment Process

### 5.1 Frontend Deployment (Vercel)
1.  **Git Integration:** We will push our `comediq-frontend` React project to a Git repository (e.g., GitHub, GitLab, Bitbucket).
2.  **Vercel Connection:** We will connect our Vercel account to this Git repository.
3.  **Automatic Deployment:** Every time we push new code to the `main` branch (or another configured branch), Vercel will automatically detect the changes, build our React application, and deploy it to their global CDN, making it accessible via a unique URL. Vercel handles SSL certificates automatically.

### 5.2 Backend Deployment (Render.com)
1.  **Git Integration:** We will push our `comediq-backend` Node.js/Express project (including the `comediq.db` SQLite file if it exists, or handling its creation on first run) to a separate Git repository.
2.  **Render Connection:** We will connect our Render account to this Git repository.
3.  **Service Creation:** We will create a "Web Service" on Render, pointing it to our Node.js repository. Render will detect the Node.js project and deploy it.
    * **SQLite Persistence:** Render's free tier for web services allows writing to the file system, which supports SQLite databases. We will need to be mindful of how we handle the `comediq.db` file to ensure its persistence across deployments and restarts if relying solely on Render's ephemeral storage without a separate disk. For simplicity, initially, we'll rely on the default behavior, but we might need to add a small script to initialize the DB if it's lost.
4.  **Environment Variables:** We'll use environment variables on Render to store sensitive information (e.g., database connection string, if we were to move to a non-SQLite database) and to define the port our Node.js server listens on.

This systematic approach ensures that ComediQ is built with clarity, maintainability, and our core principles of simplicity and cost-effectiveness in mind.
