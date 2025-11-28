# 🚀 Discover Dollar Assignment – MEAN Stack
### Dockerized Application with CI/CD on AWS EC2

![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)

## 📖 Project Summary

This project is a complete **MEAN stack application** (MongoDB, Express, Angular, Node.js) designed to manage tutorials. The entire system is fully containerized using Docker, orchestration is handled by Docker Compose, and deployment to an **AWS EC2** instance is fully automated using **GitHub Actions**.

**Key Features:**
* Create, Read, Update, and Delete (CRUD) tutorials.
* Frontend served via **Nginx** (Reverse Proxy).
* **Zero-downtime** automated deployments.
* Production-ready folder structure.

---

## 🏗️ Architecture & Changes Implemented

To transition this application from a local development setup to a production-ready cloud deployment, the following changes were engineered:

### 1. Frontend Enhancements (Angular + Nginx)
* **Custom Nginx Config:** Added `frontend/nginx/default.conf` to handle Angular routing, caching, and reverse proxying `/api` calls to the backend.
* **Dynamic API Base URL:** Updated `tutorial.service.ts` to use a relative path (`/api/tutorials`) instead of a hardcoded localhost URL. This allows Nginx to route traffic dynamically within the Docker network.
* **Dockerization:** Created a multi-stage Dockerfile to build the Angular app and serve the static files via Nginx.

### 2. Backend Enhancements (Node.js + MongoDB)
* **Containerized Database Connection:** Updated `db.config.js` to connect to `mongodb://mongo:27017` instead of `localhost`. This ensures the backend can communicate with the MongoDB container within the internal Docker network.

### 3. Root Configuration (Orchestration & CI/CD)
* **Docker Compose:** Added `docker-compose.yml` to orchestrate the `backend`, `frontend`, and `mongo` services.
* **GitHub Actions Workflow:** Added `.github/workflows/deploy.yml`. This pipeline:
    1.  Builds Frontend & Backend images.
    2.  Pushes images to Docker Hub.
    3.  Connects to AWS EC2 via SSH.
    4.  Pulls the latest code and images.
    5.  Redeploys the application automatically.

---

## 📂 Folder Structure

```text
DISCOVER-DOLLAR-ASSIGNMENT/
│
├── backend/
│   ├── app/                # API Logic (Controllers, Models, Routes)
│   ├── config/             # DB Configuration
│   ├── Dockerfile          # Backend Image Setup
│   └── server.js           # Entry Point
│
├── frontend/
│   ├── nginx/
│   │   └── default.conf    # Nginx Proxy Configuration
│   ├── src/                # Angular Source Code
│   ├── Dockerfile          # Frontend Image Setup
│   └── angular.json
│
├── .github/
│   └── workflows/
│       └── deploy.yml      # CI/CD Pipeline Configuration
│
├── docker-compose.yml      # Service Orchestration
└── README.md



🛠️ Setup Instructions
Phase 1: AWS EC2 Preparation
Launch an Ubuntu EC2 Instance.

SSH into your instance and run the following commands to install the Docker runtime:

Bash

sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo usermod -aG docker $USER && newgrp docker
sudo systemctl enable docker
sudo systemctl start docker
Phase 2: GitHub Repository Setup
Fork or Clone this repository.

Navigate to Settings → Secrets and variables → Actions.

Add the following Repository Secrets:

Secret Name	Value
DOCKER_USERNAME	Your Docker Hub Username
DOCKER_PASSWORD	Your Docker Hub Password (or Access Token)
EC2_HOST	The Public IP address of your EC2 instance
EC2_USER	ubuntu
EC2_SSH_KEY	The content of your .pem private key file

Export to Sheets

⚠️ Note: If you restart your EC2 instance and are not using an Elastic IP, the Public IP will change. You must update the EC2_HOST secret accordingly.

Phase 3: Update Configuration
Open docker-compose.yml in the root directory.

Update the image names to match your Docker Hub username (e.g., your-username/repo-name).

🚀 How to Deploy
The deployment process is fully automated.

Make your changes locally.

Push your changes to the main branch.

Bash

git add .
git commit -m "Update application"
git push origin main
Go to the Actions tab in GitHub to watch the workflow:

Builds Docker Images 📦

Pushes to Registry ☁️

Deploys to EC2 🚀

No manual SSH entry is required after the initial setup.

🧪 Testing the Deployment
Once the GitHub Action completes successfully, access your application:

Frontend
Open your browser and navigate to:

http://YOUR_EC2_PUBLIC_IP
Backend API Health Check
You can test the API directly using curl:

Bash

curl http://YOUR_EC2_PUBLIC_IP/api/tutorials
📝 Final Notes
This project demonstrates a production-grade workflow integrating:

✅ Full Dockerization of a MEAN stack.

✅ Automated CI/CD using GitHub Actions.

✅ Nginx Reverse Proxy for clean routing and security.

✅ Self-Healing Deployment (Automatic pulls of latest code/images).

Ready for immediate deployment.








