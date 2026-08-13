Store Rating Platform
A full-stack web application that lets users discover and rate registered stores. Built as a coding challenge submission, the app implements three distinct roles — System Administrator, Normal User, and Store Owner — behind a single login, with role-based access control enforced on both the client and the server.
Screenshots / Demo
> _Add screenshots or a live demo link here._
Login	User Dashboard	Admin Dashboard
screenshot	screenshot	screenshot
Tech Stack
Layer	Technology
Frontend	React (Vite)
Backend	Express.js
ORM	Sequelize
Database	PostgreSQL
Auth	JWT (JSON Web Tokens)
Project Structure
```
store-rating-app/
├── backend/     Express API, JWT auth, Sequelize models
└── frontend/    React (Vite) SPA
```
Features
Single login, three roles — Admin, Normal User, and Store Owner all authenticate through the same flow, with the UI and permissions adapting to the logged-in role.
Admin control panel — create additional admins, normal users, and stores; assign a store to a Store Owner by user ID; view platform-wide stats.
Store browsing and ratings — normal users can browse all registered stores and submit a 1–5 rating for each.
Owner insights — store owners see the ratings submitted for their store.
Server + client-side validation — every input is validated on both ends, so the rules below are always enforced regardless of how a request is made.
Getting Started
1. Prerequisites
Node.js v18+
PostgreSQL
2. Database
Create an empty PostgreSQL database:
```sql
CREATE DATABASE store_rating_db;
```
Sequelize's `sync()` auto-creates all tables on first run — no manual migrations needed for this challenge.
3. Backend Setup
```bash
cd backend
npm install
cp .env.example .env
# edit .env with your DB credentials and a JWT_SECRET
npm run seed     # creates the first admin account
npm run dev       # starts on http://localhost:5000
```
Seeded admin login:
email: `admin@storerating.com`
password: `Admin@1234`
> ⚠️ **Change this password immediately after your first login.** These credentials are shared in this README for reviewer convenience and should never be used as-is in a real deployment.
4. Frontend Setup
```bash
cd frontend
npm install
npm run dev       # starts on http://localhost:5173
```
The Vite dev server proxies `/api` requests to `http://localhost:5000`, so both servers must be running together.
How Roles Work
Admin logs in with the seeded account, then can create more admins, normal users, and stores (optionally assigning a store to a Store Owner user by their user ID).
Store Owner accounts are created by the admin (role: `owner`) and linked to a store via `ownerId`. They log in with the credentials the admin set and see their store's ratings.
Normal users self-register via the Sign Up page.
API Overview
Method	Endpoint	Access
POST	`/api/auth/signup`	Public
POST	`/api/auth/login`	Public
PUT	`/api/auth/update-password`	Any logged-in
GET	`/api/admin/dashboard`	Admin
POST	`/api/admin/users`	Admin
GET	`/api/admin/users`	Admin
GET	`/api/admin/users/:id`	Admin
POST	`/api/admin/stores`	Admin
GET	`/api/admin/stores`	Admin
GET	`/api/stores`	Normal User
POST	`/api/stores/:storeId/rating`	Normal User
GET	`/api/owner/dashboard`	Store Owner
Validation Rules
Enforced identically on the client and the server:
Field	Rule
Name	20–60 characters
Address	Max 400 characters
Password	8–16 characters, at least 1 uppercase letter + 1 special character
Email	Standard email format
Rating	Integer, 1–5
Data Model
User — `id`, `name`, `email`, `password` (hashed), `address`, `role` (`admin` | `user` | `owner`)
Store — `id`, `name`, `email`, `address`, `ownerId` (links to a User with role `owner`)
Rating — `id`, `userId`, `storeId`, `value` (1–5); one rating per user per store
License
This project was built for a coding challenge submission.
