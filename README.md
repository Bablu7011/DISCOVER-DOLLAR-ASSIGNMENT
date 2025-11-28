# 🚀 DISCOVER DOLLAR ASSIGNMENT – MEAN STACK
### Docker, GitHub Actions, AWS EC2

![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)

This project is a complete **MEAN stack application**. The whole system is fully containerized using Docker, and the deployment is automated using GitHub Actions. The application is hosted on an **AWS EC2 Ubuntu instance**.

The frontend is served through Nginx inside the frontend Dockerfile, so a separate Nginx service is not needed in `docker-compose`.

---

## 1. Project Summary

This application allows users to:
* Add a tutorial
* View tutorials
* Update a tutorial
* Delete a tutorial

**Technologies used:**
* Angular
* Node.js and Express
* MongoDB
* Nginx
* Docker & Docker Compose
* GitHub Actions
* AWS EC2

---

## 2. Changes and Improvements Made

### A. Frontend Changes

**1. Added Nginx configuration**
* **File added:** `frontend/nginx/default.conf`
* **Purpose:**
    * Handles Angular routing.
    * Acts as a reverse proxy for the backend API.
    * Provides cache control for fresh builds.

**2. Updated API URL inside Angular service**
* **File:** `frontend/src/app/services/tutorial.service.ts`
* **Change:**
    * *From:* `const baseUrl = 'http://localhost:8080/api/tutorials';`
    * *To:* `const baseUrl = '/api/tutorials';`
* **Reason:** This makes the API calls work through Nginx inside the frontend container.

**3. Frontend Dockerfile added**
* A complete Dockerfile was created to build the Angular app and serve it using Nginx.

### B. Backend Changes

**1. Backend Dockerfile added**
* Used to containerize the Node.js server.

**2. MongoDB connection updated**
* **File:** `backend/app/config/db.config.js`
* **Change:**
    * *From:* `url: "mongodb://localhost:27017/dd_db"`
    * *To:* `url: "mongodb://mongo:27017/tutorial_db"`
* **Reason:** This makes the backend connect to MongoDB inside Docker.

### C. Root Folder Additions

**1. `docker-compose.yml` created**
It contains three services:
* `backend`
* `frontend` (with Nginx inside its own image)
* `mongo`

**2. GitHub Actions workflow added**
* **File:** `.github/workflows/deploy.yml`
* **Workflow steps:**
    1.  Builds backend and pushes to Docker Hub.
    2.  Builds frontend with `--no-cache` and pushes to Docker Hub.
    3.  Connects to EC2.
    4.  Pulls the latest GitHub code.
    5.  Pulls updated Docker images.
    6.  Runs `docker-compose`.
    7.  Deploys automatically.

---

## 3. EC2 Setup Instructions

SSH to your EC2 instance and run the following commands:

```bash
sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo usermod -aG docker $USER && newgrp docker
sudo systemctl enable docker
sudo systemctl start docker
The server is now ready to deploy the application.

4. GitHub Secrets Required
Add the following secrets in: GitHub → Repo → Settings → Secrets → Actions

Secret	Description
DOCKER_USERNAME	Your Docker Hub username
DOCKER_PASSWORD	Your Docker Hub password or token
EC2_HOST	Your EC2 public IP address
EC2_USER	ubuntu
EC2_SSH_KEY	Contents of your .pem key

Export to Sheets

Note: If the EC2 instance is restarted and you have not used an Elastic IP, you have to update the EC2_HOST secret with the new public IP every time.

5. Running This Project From Scratch
If someone new wants to run this project, these are the steps:

Step 1 — Launch an EC2 Ubuntu instance Install Docker and Docker Compose using the commands mentioned in Section 3.

Step 2 — Fork or clone this repository locally

Bash

git clone [https://github.com/YOUR_USERNAME/DISCOVER-DOLLAR-ASSIGNMENT.git](https://github.com/YOUR_USERNAME/DISCOVER-DOLLAR-ASSIGNMENT.git)
Step 3 — Create a GitHub repository and push the project Make sure to update your DockerHub username in docker-compose.yml.

Step 4 — Set all required GitHub Secrets Add: DOCKER_USERNAME, DOCKER_PASSWORD, EC2_HOST, EC2_USER, EC2_SSH_KEY.

Step 5 — Commit and push your changes to the main branch As soon as you push, GitHub Actions will automatically:

Build frontend and backend Docker images

Push them to DockerHub

SSH into EC2

Pull the latest code

Run docker-compose

Deploy the application

Everything happens automatically without manual steps.

6. Folder Structure
Plaintext

DISCOVER-DOLLAR-ASSIGNMENT/
│
├── backend/
│   ├── app/
│   ├── Dockerfile
│
├── frontend/
│   ├── nginx/default.conf
│   ├── src/
│   ├── Dockerfile
│
├── docker-compose.yml
│
└── .github/workflows/deploy.yml
7. How to Test After Deployment
From your local machine:

Check backend container:

Bash

curl http://YOUR_EC2_PUBLIC_IP/api/tutorials
Check frontend: Open in browser:

Plaintext

http://YOUR_EC2_PUBLIC_IP
8. Final Notes
This project demonstrates:

✅ Complete Dockerization of a MEAN application

✅ Backend API reverse-proxying through Nginx

✅ Fully automated CI/CD pipeline using GitHub Actions

✅ Zero manual deployment

✅ Auto Git pull and auto Docker image pull on EC2

✅ Clean routing for Angular applications
