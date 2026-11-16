# National Dealership Review Portal

**Repository Name:** `xrwvm-fullstack_developer_capstone`  
**Project Name:** National Dealership Review Portal  
**Certification:** IBM Full Stack Cloud Developer Capstone  

---

## Project Description

The **National Dealership Review Portal** is a full-stack, microservices-based web application allowing customers across the United States to browse national car dealerships, view dealership inventory, read real customer reviews, submit vehicle purchase and service reviews with automated sentiment analysis, and manage customer accounts.

---

## Main Features

- **User Management & Authentication:** Secure user registration, authentication, login, session persistence, and logout powered by Django authentication.
- **Dealership Directory:** National dealership search and filtering by state and unique dealer ID.
- **Customer Reviews & Sentiment Analysis:** Real-time sentiment classification (Positive, Neutral, Negative) for reviews using Natural Language Toolkit (NLTK) and VADER sentiment analyzer microservices.
- **Review Submission:** Interactive forms allowing authenticated users to submit dealer reviews, associate vehicle make/model details, and specify purchase records.
- **Car Inventory Management:** Relational Django models managing car makes, models, vehicle types (Sedan, SUV, Wagon), and model years.
- **Responsive Modern UI:** Fast, reactive Single Page Application (SPA) frontend built with React, styled using custom CSS and Bootstrap.

---

## Technology Stack

- **Web Frontend:** React 18, React Router v6, HTML5, CSS3, Bootstrap 5.
- **Django Application Server:** Python 3, Django 6 (Core web framework, templates, user management, and car inventory APIs).
- **Dealership & Review Microservice:** Node.js, Express.js, Mongoose ODM.
- **Database Systems:** 
  - **MongoDB:** Non-relational database for dealerships and review records.
  - **SQLite:** Relational database for Django users, car makes, and car models.
- **Sentiment Analysis Microservice:** Python Flask, NLTK (Natural Language Toolkit), VADER.
- **Containerization & CI/CD:** Docker, Docker Compose, GitHub Actions, IBM Cloud Code Engine.

---

## Architecture Overview

```text
+-------------------------------------------------------------------------+
|                              Web Browser                                |
|           (React Frontend UI & Django Static Pages / Templates)         |
+------------------------------------+------------------------------------+
                                     |
                                     v
+-------------------------------------------------------------------------+
|                       Django Web Application                            |
|             (Port 8000 - Authentication, SQLite, Car Inventory)         |
+--------------------+-------------------------------+--------------------+
                     |                               |
                     v                               v
+-----------------------------------+   +---------------------------------+
|   Node.js / Express Microservice  |   |  Sentiment Analysis Service     |
|   (Port 3030 - Dealerships/Reviews) |   |  (Port 5050 - Flask & NLTK)     |
+--------------------+--------------+   +---------------------------------+
                     |
                     v
+-----------------------------------+
|          MongoDB Database         |
|         (Port 27017 - Mongoose)   |
+-----------------------------------+
```

---

## Project Structure

```text
xrwvm-fullstack_developer_capstone/
├── .github/
│   └── workflows/        # GitHub Actions CI/CD workflows
├── server/
│   ├── database/         # Node.js/Express Mongoose service & MongoDB seeds
│   │   ├── data/         # dealerships.json & reviews.json
│   │   ├── app.js        # Express API endpoints
│   │   ├── dealership.js # Dealership schema
│   │   └── review.js     # Review schema
│   ├── djangoapp/        # Django application (models, views, urls, restapis)
│   │   ├── microservices/# Sentiment analysis Flask microservice
│   │   ├── models.py     # CarMake and CarModel definitions
│   │   ├── populate.py   # Data seeding for makes and models
│   │   └── views.py      # Auth, proxy, and inventory API endpoints
│   ├── djangoproj/       # Django project configuration & settings
│   └── frontend/         # React SPA client application & static pages
│       ├── public/
│       ├── src/          # React components (Login, Register, Dealers, Header)
│       └── static/       # Static pages (About.html, Contact.html, Home.html)
├── submission/           # Graded module submissions and verification evidence
└── README.md
```

---

## Setup and Installation Instructions

### 1. Prerequisites
- Python 3.10+
- Node.js (v18+) and npm
- MongoDB Community Server or Docker Desktop

### 2. Django Backend Environment Setup
```bash
cd server
python -m venv djangoenv

# Windows:
.\djangoenv\Scripts\activate
# Linux/macOS:
source djangoenv/bin/activate

pip install -U -r requirements.txt
python manage.py makemigrations
python manage.py migrate --run-syncdb
```

### 3. Frontend Build Setup
```bash
cd server/frontend
npm install
npm run build
```

### 4. Running the Microservices
#### Option A: Using Docker Compose
```bash
cd server/database
docker-compose up --build
```
#### Option B: Running Locally
- Start MongoDB locally on port 27017.
- Start Node API:
  ```bash
  cd server/database
  npm install
  node app.js
  ```
- Start Sentiment Analyzer:
  ```bash
  cd server/djangoapp/microservices
  pip install -r requirements.txt
  python -m flask run --port=5050
  ```

### 5. Running the Django Application Server
```bash
cd server
python manage.py runserver 8000
```
Visit the application at: `http://localhost:8000`

---

## API Endpoints Reference

### Django Application Endpoints (`http://localhost:8000`)
- `POST /djangoapp/register` - Register a new user account.
- `POST /djangoapp/login` - Authenticate an existing user.
- `GET /djangoapp/logout` - Terminate active user session.
- `GET /djangoapp/get_cars` - Fetch all car models and their associated car makes.
- `GET /djangoapp/get_dealers` - Proxy endpoint for national dealership list.
- `GET /djangoapp/get_dealers/:state` - Proxy endpoint for dealerships in a specific state.
- `GET /djangoapp/dealer/:id` - Proxy endpoint for dealer details by ID.
- `GET /djangoapp/reviews/dealer/:id` - Proxy endpoint for dealership reviews with sentiment scores.
- `POST /djangoapp/add_review` - Submit a review for a dealership.

### Node.js / Express Dealership & Review Service (`http://localhost:3030`)
- `GET /fetchDealers` - Retrieve all dealerships.
- `GET /fetchDealers/:state` - Filter dealerships by state.
- `GET /fetchDealer/:id` - Retrieve a specific dealership by numerical ID.
- `GET /fetchReviews` - Retrieve all reviews.
- `GET /fetchReviews/dealer/:id` - Retrieve all customer reviews for a given dealership ID.
- `POST /insert_review` - Insert a new review document into MongoDB.

### Sentiment Analyzer Microservice (`http://localhost:5050`)
- `GET /analyze/:text` - Compute polarity scores and classify text sentiment (`positive`, `neutral`, `negative`).

---

## Public Repository Links
- **Repository:** https://github.com/Divya-Asnani/xrwvm-fullstack_developer_capstone
- **README.md:** https://github.com/Divya-Asnani/xrwvm-fullstack_developer_capstone/blob/main/README.md
- **About Us Page:** https://github.com/Divya-Asnani/xrwvm-fullstack_developer_capstone/blob/main/server/frontend/static/About.html
- **Contact Us Page:** https://github.com/Divya-Asnani/xrwvm-fullstack_developer_capstone/blob/main/server/frontend/static/Contact.html
- **Register Component:** https://github.com/Divya-Asnani/xrwvm-fullstack_developer_capstone/blob/main/server/frontend/src/components/Register/Register.jsx
