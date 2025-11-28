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








DISCOVER DOLLAR ASSIGNMENT – MEAN STACK (Docker, GitHub Actions, AWS EC2)



This project is a complete MEAN stack application.

The whole system is fully containerized using Docker, and the deployment is automated using GitHub Actions.

The application is hosted on an AWS EC2 Ubuntu instance.



The frontend is served through Nginx inside the frontend Dockerfile, so a separate Nginx service is not needed in docker-compose.



1. Project Summary



This application allows users to:



Add a tutorial



View tutorials



Update a tutorial



Delete a tutorial



Technologies used:



Angular



Node.js and Express



MongoDB



Nginx



Docker



Docker Compose



GitHub Actions



AWS EC2



2. Changes and Improvements Made

A. Frontend Changes



Added Nginx configuration



File added:



frontend/nginx/default.conf





This file handles:



Angular routing



Reverse proxy for backend API



Cache control for fresh builds



Updated API URL inside Angular service



File:



frontend/src/app/services/tutorial.service.ts





Changed from:



const baseUrl = 'http://localhost:8080/api/tutorials';





To:



const baseUrl = '/api/tutorials';





This makes the API calls work through Nginx inside the frontend container.



Frontend Dockerfile added



A complete Dockerfile was created to:



Build Angular app



Serve it using Nginx



B. Backend Changes



Backend Dockerfile added



Used to containerize the Node.js server.



MongoDB connection updated



In:



backend/app/config/db.config.js





Updated:



url: "mongodb://localhost:27017/dd_db"





To:



url: "mongodb://mongo:27017/tutorial_db"





This makes backend connect to MongoDB inside Docker.



C. Root Folder Additions



docker-compose.yml created



It contains three services:



backend



frontend (with Nginx inside its own image)



mongo







GitHub Actions workflow added



File:



.github/workflows/deploy.yml





This workflow:



Builds backend and pushes to Docker Hub



Builds frontend with --no-cache and pushes to Docker Hub



Connects to EC2



Pulls the latest GitHub code



Pulls updated Docker images



Runs docker-compose



Deploys automatically



3. EC2 Setup Instructions



SSH to your EC2 instance and run:



sudo apt update

sudo apt install docker.io -y

sudo apt install docker-compose -y

sudo usermod -aG docker $USER && newgrp docker

sudo systemctl enable docker

sudo systemctl start docker





The server is now ready to deploy the application.



4. GitHub Secrets Required



Add the following secrets in:



GitHub → Repo → Settings → Secrets → Actions



Secret Description

DOCKER_USERNAME Your Docker Hub username

DOCKER_PASSWORD Your Docker Hub password or token

EC2_HOST Your EC2 public IP address

EC2_USER ubuntu

EC2_SSH_KEY Contents of your .pem key



Note:

If EC2 instance is restarted and i have not use Elastic IP, i have to update the EC2_HOST secret with the new public IP every time.



5. Running This Project From Scratch



If someone new wants to run this project, these are the steps:



Step 1 — Launch an EC2 Ubuntu instance



Install Docker and Docker Compose using the commands mentioned earlier.



Step 2 — Fork or clone this repository locally

git clone https://github.com/YOUR_USERNAME/DISCOVER-DOLLAR-ASSIGNMENT.git



Step 3 — Create a GitHub repository and push the project



Make sure to update your DockerHub username in:



docker-compose.yml



Step 4 — Set all required GitHub Secrets



Add:



DOCKER_USERNAME



DOCKER_PASSWORD



EC2_HOST



EC2_USER



EC2_SSH_KEY



Step 5 — Commit and push your changes to the main branch



As soon as you push, GitHub Actions will automatically:



Build frontend and backend Docker images



Push them to DockerHub



SSH into EC2



Pull the latest code



Run docker-compose



Deploy the application



Everything happens automatically without manual steps.



6. Folder Structure

DISCOVER-DOLLAR-ASSIGNMENT/

│

├── backend/

│ ├── app/

│ ├── Dockerfile

│

├── frontend/

│ ├── nginx/default.conf

│ ├── src/

│ ├── Dockerfile

│

├── docker-compose.yml

│

└── .github/workflows/deploy.yml



7. How to Test After Deployment



From your local machine:



Check backend container:

curl http://YOUR_EC2_PUBLIC_IP/api/tutorials



Check frontend:



Open in browser:



http://YOUR_EC2_PUBLIC_IP



8. Final Notes



This project demonstrates:



Complete Dockerization of a MEAN application



Backend API reverse-proxying through Nginx



Fully automated CI/CD pipeline using GitHub Actions



Zero manual deployment



Auto Git pull and auto Docker image pull on EC2



Clean routing for Angular applications



Everything is set to be production-ready and easy to extend.



do not remove and add anything write as it is but in formating like you did because folder whne unstructured and text is not with point so its not a readable format so



so now write update README.md that makes this on github readme proper firmat




Show thinking
Here is the text you provided, preserved exactly in content but reformatted into a clean, professional, and structured README.md file suitable for GitHub.

Markdown

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

Note: In the EC2 instance i have not used an Elastic IP, i have to update the EC2_HOST secret with the new public IP every time and i have used t2.micro so it works slow but you can upgrade the ec2 instance type for fast repsponse, because of going to get bil i have not used an Elastic IP and used t2.micro.

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

