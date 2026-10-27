# Threadora — Full-Stack Fashion Store

A student-friendly full-stack clothing store project built with:
- **Frontend:** React + Vite, HTML, CSS, JavaScript
- **Backend:** Node.js + Express
- **Database:** MongoDB with Mongoose
- **MongoDB GUI:** MongoDB Compass

## Features
- Login-first landing page with sign-in, demo registration, and guest access
- Multi-section navigation: Home, Shop, About, Orders shortcut, and Logout
- Fashion storefront with category filters and search
- Product cards, prices, sizes, colors, and stock
- Add to cart, change quantities, remove items
- Checkout form creates an order in MongoDB
- Admin-style product management: add and delete products
- REST API with validation and useful error messages
- Seed script to insert sample fashion products
- Responsive layout for desktop and mobile

> This is an academic/demo project. Login/register is a frontend-only demo session (not secure server authentication); checkout creates a database order and does **not** process real payments.

---

## 1. Requirements
Install:
1. Node.js LTS (includes npm): https://nodejs.org/
2. MongoDB Community Server **or** a MongoDB Atlas database: https://www.mongodb.com/try/download/community
3. MongoDB Compass: https://www.mongodb.com/try/download/compass

If using local MongoDB, start the MongoDB service before running the project.

## 2. Connect MongoDB Compass
For a typical local MongoDB installation:
1. Open **MongoDB Compass**.
2. Paste this connection string into **New Connection**:
   ```text
   mongodb://127.0.0.1:27017
   ```
3. Click **Connect**.
4. The app uses the database named `threadora_fashion`. Collections `products` and `orders` are created when data is inserted.

The backend connection string is set in `backend/.env` as:
```env
MONGODB_URI=mongodb://127.0.0.1:27017/threadora_fashion
```
Compass can connect to the server using `mongodb://127.0.0.1:27017`; the app connects directly to the `threadora_fashion` database.

### If you use MongoDB Atlas instead
Create a database user, allow your IP in Network Access, copy the application connection string, and put it in `backend/.env`, for example:
```env
MONGODB_URI=mongodb+srv://YOUR_USERNAME:YOUR_PASSWORD@YOUR_CLUSTER.mongodb.net/threadora_fashion?retryWrites=true&w=majority
```
Replace placeholders. Do not share your database password or commit `.env` to GitHub.

## 3. Configure the backend
Open a terminal in the project folder:

```bash
cd backend
```

Create your environment file:
- Windows CMD:
  ```bat
  copy .env.example .env
  ```
- PowerShell:
  ```powershell
  Copy-Item .env.example .env
  ```
- macOS/Linux:
  ```bash
  cp .env.example .env
  ```

Install dependencies:
```bash
npm install
```

Seed sample products (run once, or whenever you want to reset the demo catalogue):
```bash
npm run seed
```

Start the API:
```bash
npm run dev
```

Expected message:
```text
MongoDB connected: threadora_fashion
THREADORA API running at http://localhost:5000
```

API health check: http://localhost:5000/api/health

## 4. Start the React frontend
Open a **second terminal** in the project folder:

```bash
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite, usually http://localhost:5173.

## 5. Verify MongoDB integration
1. Keep the backend running and submit a checkout order in the frontend.
2. In Compass, connect to `mongodb://127.0.0.1:27017`.
3. Refresh the databases list.
4. Open `threadora_fashion` → `orders` to see the order document.
5. Open `threadora_fashion` → `products` to see catalogue documents.

## API routes
| Method | Route | Purpose |
|---|---|---|
| GET | `/api/health` | API and database status |
| GET | `/api/products` | List products; supports `?search=` and `?category=` |
| GET | `/api/products/:id` | Get one product |
| POST | `/api/products` | Add product |
| DELETE | `/api/products/:id` | Delete product |
| POST | `/api/orders` | Create an order in MongoDB |
| GET | `/api/orders` | List recent orders (demo/admin use) |

## Project structure
```text
Threadora_Fashion_Store/
├── README.md
├── backend/
│   ├── .env.example
│   ├── package.json
│   ├── server.js
│   ├── models/
│   │   ├── Product.js
│   │   └── Order.js
│   └── scripts/seed.js
└── frontend/
    ├── package.json
    ├── index.html
    ├── vite.config.js
    └── src/
        ├── main.jsx
        ├── App.jsx
        ├── api.js
        └── styles.css
```

## Troubleshooting
- **ECONNREFUSED 127.0.0.1:27017:** MongoDB Server is not running. Start the MongoDB service, then restart the backend.
- **Compass connects but app doesn't:** Check `backend/.env`, especially `MONGODB_URI`.
- **Products don't appear:** Run `npm run seed` inside `backend`, and confirm the API is running.
- **Frontend can't reach API:** Ensure backend runs on port 5000. Vite proxies `/api` to it.
- **Port already in use:** Change `PORT` in `backend/.env` and update the Vite proxy if needed.

## Security note
This starter is for local coursework. Product creation/deletion and order listing are intentionally unauthenticated for demo simplicity. Add authentication, authorization, rate limiting, and payment-provider integration before using it publicly.


## Login-first pages
The app starts at a fashion-themed login page. Users can switch to the demo registration form or continue as a guest. After entering the storefront, the bottom navigation provides Home, Shop, About, and Orders access; the header includes Log out. The demo login does not persist accounts or send passwords to the backend. For a production project, implement server-side authentication with hashed passwords, sessions/JWT, and protected routes.
