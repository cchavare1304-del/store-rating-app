\# Store Rating Platform



Full-stack app for rating registered stores. Three roles: System Administrator,

Normal User, Store Owner — one login, role-based access.



\*\*Stack:\*\* Express + Sequelize (backend) · PostgreSQL (DB) · React + Vite (frontend)



\## Project Structure

store-rating-app/

├── backend/ Express API, JWT auth, Sequelize models

└── frontend/ React (Vite) SPA



\## 1. Database



Create a PostgreSQL database:

```sql

CREATE DATABASE store\_rating\_db;

```



\## 2. Backend Setup



```bash

cd backend

npm install

cp .env.example .env

\# edit .env with your DB credentials and a JWT\_SECRET

npm run seed     # creates the first admin account

npm run dev       # starts on http://localhost:5000

```



Seeded admin login:

\- email: `admin@storerating.com`

\- password: `Admin@1234`



\*\*Change this password after first login.\*\* Sequelize's `sync()` auto-creates

tables on first run — no manual migration needed for this challenge.



\## 3. Frontend Setup



```bash

cd frontend

npm install

npm run dev       # starts on http://localhost:5173

```



The Vite dev server proxies `/api` requests to `http://localhost:5000`, so run

both servers together.



\## How Roles Work



\- \*\*Admin\*\* logs in with the seeded account, then can create more admins,

&#x20; normal users, and stores (optionally assigning a store to a Store Owner

&#x20; user by their user ID).

\- \*\*Store Owner accounts\*\* are created by the admin (role: `owner`) and

&#x20; linked to a store via `ownerId`. They log in with the credentials the

&#x20; admin set and see their store's ratings.

\- \*\*Normal users\*\* self-register via the Sign Up page.



\## API Overview



| Method | Endpoint                          | Access        |

|--------|------------------------------------|---------------|

| POST   | /api/auth/signup                   | Public        |

| POST   | /api/auth/login                    | Public        |

| PUT    | /api/auth/update-password          | Any logged-in |

| GET    | /api/admin/dashboard               | Admin         |

| POST   | /api/admin/users                   | Admin         |

| GET    | /api/admin/users                   | Admin         |

| GET    | /api/admin/users/:id                | Admin         |

| POST   | /api/admin/stores                  | Admin         |

| GET    | /api/admin/stores                  | Admin         |

| GET    | /api/stores                        | Normal User   |

| POST   | /api/stores/:storeId/rating        | Normal User   |

| GET    | /api/owner/dashboard                | Store Owner   |



\## Validation Rules (enforced both client + server)

\- Name: 20-60 characters

\- Address: max 400 characters

\- Password: 8-16 characters, at least 1 uppercase letter + 1 special character

\- Email: standard format

\- Rating: integer 1-5

