# ComediQ MVP Documentation Checklist

This document tracks the components we have and plan to build for the ComediQ Minimum Viable Product (MVP).

## Current Status: Planning & Initial Setup

## Tech Stack Overview

* **Frontend:**
    * React (via Create React App)
    * HTML5
    * CSS3
    * Plain JavaScript (ES6+)
* **Backend:**
    * Node.js
    * Express.js
    * SQLite3 (Database)
    * `node-sqlite3` (Node.js driver for SQLite3)
* **Deployment:**
    * Vercel (for Frontend)
    * Render.com (for Backend)

---

## MVP Features Checklist

### Core Application Structure
- [ ] React Application Setup (Create React App)
- [ ] Basic HTML/CSS/JS Structure
- [ ] Node.js/Express Backend Setup
- [ ] SQLite Database Integration

### UI Layout
- [ ] Bottom Tab Navigation Component (Home, Find Mics, Schedule, Recordings, Data)

### Pages & Core Functionality

#### 1. Home Page
- [ ] Basic placeholder for relevant user info (upcoming spots, mics near you, recent data)

#### 2. Find Open Mics Page
- [ ] **Tab Structure:** Map View / List View
- [ ] **List View:**
    - [ ] Basic Mic Listing Display (Mic Name, Frequency, Status, Time, Location)
    - [ ] Placeholder for detailed mic attributes (Cost, Time Spent, Quality, Rules)
    - [ ] Like/Dislike Buttons (Frontend interaction, data not yet stored)
    - [ ] **Subtabs:**
        - [ ] Active View (Placeholder for location-based filtering within 4 hours)
        - [ ] Glance View (Placeholder for 3-metric display)
        - [ ] Default View (Placeholder for 6 mics/page, ordered by start time)
- [ ] **Map View:**
    - [ ] Basic map integration placeholder (no actual map API integration yet to minimize initial complexity)
    - [ ] Pinpoint locations (static data representation)

#### 3. Mic Scheduler Page
- [ ] Placeholder for basic calendar view/list
- [ ] Placeholder for adding selected mics to schedule
- [ ] Placeholder for exporting to Google Calendar
- [ ] Placeholder for adding shows, writing, editing sessions, etc.
- [ ] Placeholder for auto-suggested hang time and commute times

#### 4. List a Mic (Admin/User Submission)
- [ ] Basic form to submit new mic details (initially, this might be a simple form that only adds to SQLite, not full validation)

#### 5. Community Tab
- [ ] Placeholder for Comedy Circles
- [ ] Placeholder for Mic Exchange System
- [ ] Placeholder for Community Building Tools

#### 6. Track Your Comedy Data
- [ ] Placeholder for Spot Counter (display total mics done)
- [ ] Placeholder for viewing past mics and jokes
- [ ] Placeholder for basic Stats dashboard (Day/Week/Month/Year/Career views)

#### 7. Locked Tab
- [ ] Grayed out UI for Sections (Recordings, Parallel Thinking, ComedyCloud, Comedian Content Portfolio, Progress Tracker Gamification)
- [ ] Basic 1-5 ranking vote UI (frontend interaction, data not yet stored)

---

## Future Considerations (Beyond MVP)
* User authentication and accounts (Login, Signup, Profiles)
* Location services integration for 'Find Mics'
* Actual Map API integration (e.g., Google Maps API)
* Advanced scheduling features with real-time updates
* Robust database design for scalability (potentially moving beyond SQLite)
* Image/Media uploads
* Real-time chat functionality
* AI/LLM integrations for 'Recordings' and 'Parallel Thinking Analysis'
* Patreon/OnlyFans style subscription integration
* Gamification logic and leaderboards
* Push notifications
