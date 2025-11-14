# RAG Chatbot Quick Server Setup

## 1. Update & Install Essentials

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install git curl python3 python3-pip python3-venv nginx mongodb certbot python3-certbot-nginx -y

# Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

---

## 2. Clone Your Project

```bash
cd /var/www
git clone https://github.com/Aditeya007/Admin_RAG.git rag-chatbot
cd rag-chatbot
```

---

## 3. Create `.env` File

```bash
nano .env
```

**Paste this template and fill your values:**

```env
# Database
MONGODB_URI=mongodb://localhost:27017
MONGO_URI=mongodb://localhost:27017/rag_chatbot
MONGODB_DATABASE=rag_chatbot

# Security Secrets (see below on how to generate)
JWT_SECRET=your_jwt_secret_here
FASTAPI_SHARED_SECRET=your_fastapi_secret_here

# Google AI Key
GOOGLE_API_KEY=your_google_api_key

# Service URLs
FASTAPI_BOT_URL=http://localhost:8000
CORS_ORIGIN=http://localhost:3000

NODE_ENV=production  # For live server!
PORT=5000
PYTHON_BIN=python3

# Other settings (rarely changed)
UPDATER_MONGODB_URI=mongodb://localhost:27017/
UPDATER_MONGODB_DATABASE=scraper_updater
DEFAULT_VECTOR_BASE_PATH=./tenant-vector-stores
CHROMA_DB_PATH=./test_DB
CHROMA_COLLECTION_NAME=scraped_content
DEFAULT_DATABASE_URI_BASE=mongodb://localhost:27017
DEFAULT_BOT_BASE_URL=http://localhost:8000
DEFAULT_SCHEDULER_BASE_URL=http://localhost:9000
DEFAULT_SCRAPER_BASE_URL=http://localhost:7000
SCRAPER_LOG_LEVEL=INFO
UPDATER_LOG_LEVEL=INFO
```

---

## 4. Generate Security Keys

You need **JWT_SECRET** and **FASTAPI_SHARED_SECRET**. Here’s how:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```
- Run this **twice**, copy each value:
  - First output → **JWT_SECRET**
  - Second output → **FASTAPI_SHARED_SECRET**
- Paste both into .env as shown above.

---

## 5. Get Google API Key

- Go to https://console.cloud.google.com/apis/credentials
- Create an API Key, add restrictions for your domain (if needed).
- Paste **GOOGLE_API_KEY** value in `.env`.

---

## 6. Install Dependencies and Build

**Backend:**
```bash
cd admin-backend
npm install
cd ..
```

**Frontend:**
```bash
cd admin-frontend
npm install
npm run build
cd ..
```

**Python/Bot:**
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
playwright install chromium
```

---

## 7. Start MongoDB

```bash
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

---

## 8. Start Services (Development Mode)

**Node.js backend:**
```bash
cd admin-backend
node server.js &
cd ..
```
**Bot:**
```bash
source venv/bin/activate
python BOT/app_20.py &
```

**Frontend (for testing):**
```bash
cd admin-frontend
npm start
```

---

## For Production:
- Make sure you use your **public domain** and update `.env` URLs
- Always set `NODE_ENV=production` in `.env`

---

**That’s it! Now your server is ready for launch.**
If stuck, check logs or ask your IT team.
