# StockFlow - Inventory Management System

A full-stack Inventory Management application built with **Django REST Framework** (Backend) and **React + Vite** (Frontend).

---

## 🚀 Quick Deployment Guide

### Option 1: Render (1-Click Blueprint) - Recommended

This repository includes a `render.yaml` Blueprint file that automatically configures:
- **Managed PostgreSQL Database** (`stockflow-db`)
- **Django REST API Web Service** (`stockflow-backend`) with automated migrations & static files collection
- **React Frontend Static Site** (`stockflow-frontend`) with SPA routing rewrites

#### Steps:
1. Push this repository to your GitHub account:
   ```bash
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
2. Log into [Render Dashboard](https://dashboard.render.com/).
3. Click **New +** > **Blueprint**.
4. Connect your GitHub repository.
5. Render will detect `render.yaml` and prompt you to apply the blueprint.
6. Click **Apply**. Both your database, API, and frontend will build and deploy automatically!

---

### Option 2: Render (Backend) + Vercel (Frontend)

If you prefer deploying the React frontend on Vercel:

#### 1. Deploy Backend on Render:
- Create a new **Web Service** on Render connected to your GitHub repository.
- **Root Directory**: `backend`
- **Environment**: `Python`
- **Build Command**: `./build.sh`
- **Start Command**: `gunicorn config.wsgi:application`
- **Environment Variables**:
  - `DJANGO_SECRET_KEY`: *(Generate a secure random string)*
  - `DEBUG`: `False`
  - `DATABASE_URL`: *(Your PostgreSQL connection string or Render database)*
  - `CORS_ALLOW_ALL_ORIGINS`: `True`

#### 2. Deploy Frontend on Vercel:
- Go to [Vercel](https://vercel.com/) and click **Add New Project**.
- Import your repository.
- **Root Directory**: Select `frontend`.
- **Framework Preset**: Vite (detected automatically).
- **Environment Variables**:
  - `VITE_API_BASE_URL`: `https://<your-render-backend-name>.onrender.com/api`
- Click **Deploy**. (The included `frontend/vercel.json` ensures SPA routes load without 404s).

---

### Option 3: Docker & Docker Compose (Any VPS / Cloud Server)

To run the entire stack (PostgreSQL + Django + Nginx React) locally or on a cloud virtual machine:

```bash
docker compose up --build -d
```
- Frontend will be available at: `http://localhost`
- Backend API will be available at: `http://localhost:8000`
- PostgreSQL will be running on port `5432`

---

## 📂 Project Architecture

```
├── backend/
│   ├── build.sh             # Render build & migration script
│   ├── Dockerfile           # Backend container spec
│   ├── Procfile             # Cloud process definition
│   ├── requirements.txt     # Python dependencies (UTF-8, Linux/Render compatible)
│   ├── config/              # Django settings & URL routing
│   ├── accounts/            # User auth & JWT handling
│   ├── products/            # Product catalog & stock inventory
│   └── dashboard/           # Analytics & KPI endpoints
│
├── frontend/
│   ├── vercel.json          # Vercel SPA routing rewrites
│   ├── Dockerfile           # Multi-stage production container with Nginx
│   ├── nginx.conf           # Nginx reverse proxy & fallback
│   ├── public/_redirects    # Netlify / Render Static Site redirect rules
│   └── src/
│       ├── api.js           # Resilient API client with URL normalization
│       └── App.jsx          # StockFlow React application
│
├── render.yaml              # Render Blueprint Infrastructure-as-Code
└── docker-compose.yml       # Multi-container orchestration
```
