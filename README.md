# Microloans Service — Dockerized, Environment-Based Deployment with CI/CD

This project is a fully containerized **Microloans API service** built using Python, Flask, SQLAlchemy, PostgreSQL, NGINX (HTTPS), and GitHub Actions for automated CI/CD.  
It supports **development**, **staging**, and **production** environments with automatic Docker image publishing to **GitHub Container Registry (GHCR)**.

---

# 🚀 Features

- RESTful Loan Management API  
- PostgreSQL database with Alembic migrations  
- NGINX reverse proxy (HTTPS support)  
- Docker & Docker Compose based architecture  
- Multi-environment workflow (`dev`, `staging`, `prod`)  
- Automated CI/CD with:
  - Build  
  - Test  
  - Security scan (Trivy)  
  - Push to GHCR  
- Versioned images using Git commit SHA  
- Environment-specific tagged images:
  - `dev-latest`
  - `staging-latest`
  - `prod-latest`

---

# 📦 Project Structure

.
├── app/ # Flask application
├── alembic/ # DB migrations
├── alembic.ini
├── Dockerfile # Builds API container
├── docker-compose.yml # Local environment
├── nginx.conf # HTTPS + Reverse proxy
├── certs/ # SSL certificates (local use)
├── .env.dev # Environment variables (development)
├── .env.staging # Environment variables (staging)
├── .env.prod # Environment variables (production)
└── .github/workflows/cicd.yml # CI/CD Pipeline


---

# 🐳 Running the Project Locally (Development)

### 1. Create `.env.dev`
ENVIRONMENT=DEV
FLASK_ENV=development
API_PORT=8000

DB_USER=postgres
DB_PASSWORD=postgres
DB_PORT=5432
DB_NAME=microloans_dev
POSTGRES_INIT_DB_NAME=microloans_dev
PGDATA=/var/lib/postgresql/data



### 2. Start services
```sh
docker compose --env-file .env.dev up --build
```

---

### 3. API is now available at:
http://localhost:8000
🌐 Running Staging / Production Images
Images are automatically published to:
```sh
ghcr.io/aniketdhakate007/dummy-branch-app
```
Pull images:
Dev:
```sh
docker pull ghcr.io/aniketdhakate007/dummy-branch-app:dev-latest
```
Staging:
```sh
docker pull ghcr.io/aniketdhakate007/dummy-branch-app:staging-latest
```
Production:
```sh
docker pull ghcr.io/aniketdhakate007/dummy-branch-app:prod-latest
```
Run image locally:
```sh
docker run -p 8000:8000 \
  -e DATABASE_URL="postgresql+psycopg2://postgres:postgres@host:5432/microloans_dev" \
  ghcr.io/aniketdhakate007/dummy-branch-app:dev-latest

```
🔥 API Endpoints
Base URL (local):

http://localhost:8000
1️⃣ Health Check
GET /health
Response:

json
```sh
{ "status": "ok", "message": "Service healthy" }
```
2️⃣ Create Loan
POST /loans
Request Body:

```sh
{
  "borrower_id": "12345",
  "amount": 1000,
  "duration_months": 12
}
Note: borrower_id MUST be a string.


```
3️⃣ Get All Loans
GET /loans
Response:


```sh
[
  {
    "loan_id": "abc123",
    "borrower_id": "12345",
    "amount": 1000,
    "duration_months": 12,
    "status": "PENDING"
  }
]

```
4️⃣ Get Loan by ID
GET /loans/{loan_id}
Example:

```sh
GET /loans/abc123
Response:

json

{
  "loan_id": "abc123",
  "borrower_id": "12345",
  "amount": 1000,
  "duration_months": 12,
  "status": "PENDING"
}
```
🔐 SSL/HTTPS (Production)
Place generated SSL certs in:

```sh
/certs/cert.pem
/certs/key.pem
NGINX automatically loads them and serves:

https://your-domain.com
```
🤖 CI/CD Pipeline Summary (GitHub Actions)
Located at:

```sh
.github/workflows/cicd.yml
```
Pipeline Includes:
✔ Build
✔ Docker image creation
✔ Tagging (commit-sha, dev-latest, etc.)
✔ Security scan using Trivy
✔ Push to GHCR
✔ Trigger on:

Push to main, staging, dev

Pull requests

Manual dispatch

🧪 Testing the API (Sample CURL Commands)
Create loan:
```sh
curl -X POST http://localhost:8000/loans \
  -H "Content-Type: application/json" \
  -d '{"borrower_id":"123", "amount":5000, "duration_months":6}'
Get loans:

curl http://localhost:8000/loans
Get loan by ID:

curl http://localhost:8000/loans/<id>
```


👤 Author
Aniket Dhakate
GitHub: https://github.com/aniketdhakate007
Project maintained and deployed with GitHub Actions.
