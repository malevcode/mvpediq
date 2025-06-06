# LLM Rebuild Prompt: ComediQ Frontend & Backend MVP (React, Node.js, Express, SQLite)

This prompt is designed to instruct an advanced Large Language Model to entirely rebuild the ComediQ web application's MVP frontend and backend from scratch, based on our established minimalistic tech stack and core requirements. This prompt should be used if significant bugs, code complexity, or other issues necessitate a complete reset.

---

**Prompt Context and Constraints:**

You are a highly skilled full-stack developer tasked with building the Minimum Viable Product (MVP) for "ComediQ," a comedy app. Your goal is to deliver functional code with the utmost simplicity, minimizing external dependencies and costs.

**Strict Tech Stack:**
* **Frontend:** React (using Create React App for setup), HTML5, CSS3, Plain JavaScript (ES6+).
    * **NO TypeScript.**
    * **NO outside JavaScript libraries/frameworks beyond React itself and standard browser APIs (e.g., `fetch`)** unless explicitly specified below as `RISKILY GRANTED`.
* **Backend:** Node.js (runtime), Express.js (web framework), SQLite3 (file-based database).
    * **The only permissible outside library for the backend is `sqlite3`** for database interaction.
* **Deployment Considerations:** Code should be structured for easy deployment to Vercel (frontend) and Render.com (backend).

**Code Principles:**
* **Simplicity First:** Prioritize the clearest, most straightforward implementation. Avoid over-engineering.
* **Minimalism:** Only implement features strictly necessary for the MVP as outlined.
* **Readability:** Code should be clean, well-commented, and easy for non-technical individuals (comedians) to understand its purpose at a high level.
* **Functional Components (React):** Prefer functional React components with Hooks where appropriate for state management.
* **Error Handling:** Basic error handling should be in place, but exhaustive error logging/monitoring is not required for this MVP.
* **No Authentication/Authorization:** User accounts, login, signup, and protected routes are explicitly **NOT** part of this MVP. All data will be accessible publicly for now.

---

**ComediQ MVP Core Functionality Requirements (Rebuild):**

**A. Frontend (React Application)**

1.  **Project Setup:**
    * Initialize a new React project using `create-react-app` in a directory named `comediq-frontend`.
    * Clean up default boilerplate to bare minimum.
2.  **UI Aesthetic & Styling:**
    * **Do NOT use any external CSS frameworks (e.g., Bootstrap, Tailwind CSS) or component libraries.** All styling must be custom CSS.
    * **Mimic the aesthetic feel of `www.comediq.us`:**
        * Analyze `www.comediq.us` to **extract a core color palette** (e.g., background colors, text colors, accent colors) and apply it consistently across the new application.
        * Adopt a **simple, clean, and intuitive design** that feels consistent with the professional and modern look of the existing site, without being a pixel-perfect replica. Focus on good use of whitespace, clear typography, and logical content flow.
        * Implement **basic responsive design** using CSS media queries to ensure usability on mobile and desktop screens.
3.  **Core Layout:**
    * Implement a **fixed bottom navigation bar** with 5 buttons, visually resembling the standard bottom tab navigation seen on mobile apps.
    * Buttons, in order: "Home", "Find Mics", "Schedule", "Recordings", "Data".
    * Clicking a button should switch the main content area to display the corresponding "page" component. Use **React Router DOM** (RISKILY GRANTED: Yes, for routing, this is a necessary core routing library in React as managing routes manually contradicts "simplicity" for any app beyond one page).
    * The "Recordings" and "Data" buttons (and their corresponding pages) should visually appear "locked" (grayed out) but still clickable for the voting mechanism (see "Locked Tab" below).
4.  **Pages (React Components):**
    * Create a separate React functional component for each of the 5 main pages.
    * **Home Page (`HomePage.js`):** Display a simple `<h1>` "Home" and a placeholder `div` for "Relevant content here."
    * **Find Open Mics Page (`FindMicsPage.js`):**
        * Implement two tabs: "List View" and "Map View". Use React state to toggle between these views.
        * **List View:**
            * Display a basic list of mic entries. This list will fetch data from the backend.
            * Each mic entry should have a "Like" button and a "Dislike" button. These buttons should display the current `likeCount` and `dislikeCount` fetched from the backend. Clicking should send a `POST` request to the backend and update the displayed count on success.
            * Implement three sub-tabs within the List View: "Active View", "Glance View", "Default View". Use React state to toggle between these sub-views. For this MVP, simply display the sub-tab name as `<h1>` in each respective sub-view. The actual filtering and simplified metrics for glance/active views will be backend tasks later.
        * **Map View:** Display a simple `<h1>` "Map View" and a placeholder `div` for "Map will go here." (No actual map integration required beyond a visual placeholder).
    * **Schedule Page (`SchedulePage.js`):** Display a simple `<h1>` "Schedule" and a placeholder `div` for "Your schedule details."
    * **Recordings Page (`RecordingsPage.js`):** Display a simple `<h1>` "Recordings" and a placeholder `div` for "Locked: AI analysis & recording tools." This page should visually be grayed out/locked.
    * **Data Page (`DataPage.js`):** Display a simple `<h1>` "Data" and a placeholder `div` for "Locked: Your comedy metrics." This page should visually be grayed out/locked.
5.  **Locked Tab Functionality:**
    * On the "Recordings" and "Data" pages, beneath the "Locked" message, display a simple UI for voting:
    * Sections to vote on: "Recordings Features", "Parallel Thinking Analysis", "ComedyCloud", "Comedian Content Portfolio", "Progress Tracker Gamification Comedy Levels".
    * For each section, provide input fields (e.g., dropdowns or radio buttons) allowing a user to assign a unique rank from 1 to 5, or "Skip". A section cannot have the same rank as another. This should be managed entirely in **frontend React state** for the MVP. No backend interaction.
6.  **List a Mic (Frontend Form):**
    * Create a simple form (e.g., on the `FindMicsPage` or a separate `ListMicPage`) with input fields for `name`, `frequency`, `status` (as a dropdown with Verified/Recently Active/Unverified/Cancelled options), `startTime`, `endTime`, `location`, `venueName`, `borough`, `neighborhood`, `cost`, `timeSpent`, `quality`, and `rules`.
    * Upon submission, send a `POST` request to the backend's `/api/mics` endpoint.
    * Display a clear success or error message to the user after submission.

**B. Backend (Node.js/Express.js with SQLite3)**

1.  **Project Setup:**
    * Initialize a new Node.js project in a directory named `comediq-backend`.
    * Install `express` and `sqlite3`.
2.  **Server Initialization:**
    * Create `server.js` (or `app.js`) to set up an Express server.
    * The server should listen on `PORT` from environment variables, or `5000` by default.
    * Implement **CORS headers manually** using `res.setHeader` in a middleware to allow requests from the frontend during development (e.g., `Access-Control-Allow-Origin: *`, `Access-Control-Allow-Methods: GET,POST,PUT,DELETE`, `Access-Control-Allow-Headers: Content-Type`). This avoids the `cors` npm package.
3.  **Database (SQLite3):**
    * Initialize an SQLite database file named `comediq.db` in the `comediq-backend` root directory.
    * On server startup, if `comediq.db` does not exist, it should create the database and a table named `mics` with the following columns:
        * `id` (INTEGER PRIMARY KEY AUTOINCREMENT)
        * `name` (TEXT NOT NULL)
        * `frequency` (TEXT)
        * `status` (TEXT)
        * `startTime` (TEXT)
        * `endTime` (TEXT)
        * `location` (TEXT)
        * `venueName` (TEXT)
        * `borough` (TEXT)
        * `neighborhood` (TEXT)
        * `cost` (TEXT)
        * `timeSpent` (TEXT)
        * `quality` (TEXT)
        * `rules` (TEXT)
        * `likeCount` (INTEGER DEFAULT 0)
        * `dislikeCount` (INTEGER DEFAULT 0)
    * Populate the `mics` table with at least **3 diverse dummy mic entries** upon initial creation, ensuring all columns are filled with realistic data.
4.  **API Endpoints:**
    * **GET `/api/mics`:**
        * Returns all open mic entries from the `mics` table in JSON format.
        * Sort the results by `startTime` in ascending order.
    * **POST `/api/mics`:**
        * Accepts a JSON body with `name`, `frequency`, `status`, `startTime`, `endTime`, `location`, `venueName`, `borough`, `neighborhood`, `cost`, `timeSpent`, `quality`, and `rules`.
        * Inserts the new mic into the `mics` table, initializing `likeCount` and `dislikeCount` to 0.
        * Responds with `201 Created` status and the newly created mic data (including its `id`).
        * Handle basic validation: `name` must be present. If invalid, return `400 Bad Request` with an error message.
    * **POST `/api/mics/:id/like`:**
        * Accepts `id` as a URL parameter.
        * Increments the `likeCount` for the specified mic in the database.
        * Responds with `200 OK` status and the updated mic data. If ID not found, return `404 Not Found`.
    * **POST `/api/mics/:id/dislike`:**
        * Accepts `id` as a URL parameter.
        * Increments the `dislikeCount` for the specified mic in the database.
        * Responds with `200 OK` status and the updated mic data. If ID not found, return `404 Not Found`.

**C. Frontend-Backend Integration:**

1.  **Find Mics Data Fetching:** The React `FindMicsPage` (specifically the List View) should perform a `GET` request to the backend's `/api/mics` endpoint upon initial render to populate the mic list.
2.  **Dynamic Mic Listing:** Dynamically render the fetched mic data (name, frequency, status, time, location, like/dislike counts, etc.) in the List View.
3.  **Like/Dislike Interaction:** Implement the functionality for the Like/Dislike buttons to send `POST` requests to the respective backend endpoints and update the UI accordingly.
4.  **List a Mic Form Submission:** The frontend form for listing a mic should send data to the backend's `POST /api/mics` endpoint.

**D. Code Structure & Best Practices (within constraints):**

* **Frontend:**
    * Organize components into logical directories (e.g., `src/components`, `src/pages`, `src/assets/css` for global styles).
    * Use React state for local component data and `useState`/`useEffect` hooks for component lifecycle and data fetching.
    * Use the native `fetch` API for all backend API calls.
* **Backend:**
    * Separate concerns: `server.js` for app setup, `routes/micRoutes.js` for API endpoints, `db/database.js` for database initialization and interaction logic.
    * Use promises or `async/await` for database operations to manage asynchronous code effectively.
    * Apply basic input validation on `POST` requests.

**Final Output:**

Provide the complete file structure and all necessary code files (e.g., `comediq-frontend/src/App.js`, `comediq-backend/server.js`, `comediq-backend/db/database.js`, etc.) for the ComediQ MVP as described, adhering strictly to the tech stack, code principles, and UI aesthetic guidelines. Include clear, concise instructions on how to run both the frontend and backend locally for development.
