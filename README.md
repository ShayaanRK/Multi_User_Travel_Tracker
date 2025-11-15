# Multi User Travel Tracker

A lightweight travel tracker web app that lets you manage multiple user profiles and track which countries each user has visited. Built with Express.js and EJS templates, and backed by PostgreSQL for persistent storage. Users can be selected from a dropdown, their visited countries are shown, and new countries or users can be added via simple forms.

---

## Table of Contents
- Project Overview
- Features
- Tech Stack
- Repository Structure
- Prerequisites
- Setup & Installation
- Database schema (example)
- License
- Contact

---

## Project Overview
This project demonstrates a simple multi-user travel tracking application:
- Maintain user profiles.
- Store which countries each user has visited.
- Add new users and add countries to a user's visited list.
- Simple server-rendered UI using EJS templates.

This is ideal for learning server-rendered Node.js apps, PostgreSQL integration, and basic CRUD flows.

---

## Features
- List users and view their visited countries
- Add new users (profiles)
- Add new visited countries for a selected user
- Server-rendered UI (EJS) with simple styling
- PostgreSQL-backed persistent data storage

---

## Tech Stack
- Node.js (Express)
- EJS (server-side HTML templates)
- PostgreSQL (data storage)
- CSS for styling
- (Optional) nodemon for development

---

## Repository Structure (recommended)
Below is a typical structure for this kind of project. The actual repository may vary slightly, but the intent and locations are the same.

root
│  index.js          # Express app entry point
│  .env              # Environment variables (ignored by git)
│  package.json
│  README.md
│
├─ views/            # EJS templates
│   index.ejs
│   new.ejs
│
├─ public/styles/    # Static assets
│   main.css
│   new.css
│
└─ sql/
    schema.sql       # Database initialization


(If your repo differs, treat this as the conceptual layout to help navigate.)

---

## Prerequisites
- Node.js (>= 14 recommended)
- npm (or yarn)
- PostgreSQL (local or hosted)
- psql (command line) or any DB GUI to run SQL

---

## Setup & Installation

1. Clone the repository
   ```
   git clone https://github.com/ShayaanRK/Multi_User_Travel_Tracker.git
   cd Multi_User_Travel_Tracker
   ```

2. Install dependencies
   ```
   npm install
   ```
   (If the project uses yarn: `yarn`)

3. Create environment variables

   Create a `.env` file in the project root (do NOT commit it). Example contents:

   ```
   PORT=3000
   NODE_ENV=development
   # PostgreSQL connection (example using connection string)
   DATABASE_URL=postgres://your_pg_user:your_pg_password@localhost:5432/travel_tracker_db
   # Or set individual PG_ variables instead:
   # PGUSER=your_pg_user
   # PGPASSWORD=your_pg_password
   # PGHOST=localhost
   # PGDATABASE=travel_tracker_db
   # PGPORT=5432

   SESSION_SECRET=change_this_to_a_secure_random_value
   ```

   Adjust values for your environment.

4. Create the PostgreSQL database

   Using psql:
   ```
   createdb travel_tracker_db
   ```

   Or via your DB UI, create a database named `travel_tracker_db` (or another name, then use that name in `.env`).

5. Initialize the schema

   If your repo contains a SQL schema file (for example in `/sql/schema.sql`), run it:
   ```
   psql -d travel_tracker_db -f sql/schema.sql
   ```

   If there is no schema file provided, here is a minimal example schema you can run to create tables used by the app:

   ```sql
   -- users table
   CREATE TABLE IF NOT EXISTS users (
     id SERIAL PRIMARY KEY,
    name VARCHAR(15) UNIQUE NOT NULL,
    color VARCHAR(15)
   );

   -- countries table
   CREATE TABLE IF NOT EXISTS countries (
     id SERIAL PRIMARY KEY,
     country_code CHAR(2) NOT NULL,
     user_id INTEGER REFERENCES users(id)
   );

   -- user_visits (many-to-many)
   CREATE TABLE IF NOT EXISTS user_visits (
     id SERIAL PRIMARY KEY,
     user_id INT REFERENCES users(id) ON DELETE CASCADE,
     country_id INT REFERENCES countries(id) ON DELETE CASCADE,
     visited_on DATE,
     UNIQUE (user_id, country_id)
   );
   ```

   Optionally add seed data to test the UI:
   ```sql
   INSERT INTO users (name, color)
   VALUES ('Pewds', 'teal'), ('Jack', 'powderblue');
  
   INSERT INTO visited_countries (country_code, user_id)
   VALUES ('FR', 1), ('GB', 1), ('CA', 2), ('FR', 2 );
   ```

---

## Common npm Scripts (example)
Check package.json to see what scripts are defined. Typical scripts:

- start — start the server (node server.js)
- dev — start with nodemon for development
- test — run tests (if provided)

---

## Contributing
Contributions are welcome. Typical workflow:
1. Fork the repository
2. Create a feature branch (git checkout -b feature/your-feature)
3. Make changes and add tests where appropriate
4. Open a pull request describing your changes

Please follow the existing coding style (server-rendered routes with EJS, keep UI simple).

---

## License
Specify a license for the project (e.g., MIT). If the repo already contains a LICENSE file, follow that license. Example:

MIT © Your Name

---

## Contact
If you have questions or want to collaborate, open an issue or contact the repository owner: ShayaanRK

---

Thank you for using Multi User Travel Tracker — a small and simple example for learning Express, EJS, and PostgreSQL powered apps.
