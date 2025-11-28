DISCOVER DOLLAR ASSIGNMENT – MEAN STACK (Docker, GitHub Actions, AWS EC2)

This project is a complete MEAN stack application.
The whole system runs inside Docker containers and deploys automatically using GitHub Actions.
The application is hosted on an AWS EC2 Ubuntu instance.

The frontend is served through Nginx that is built directly inside the frontend Dockerfile.

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
1. Nginx configuration added

File: frontend/nginx/default.conf

It handles:

Angular routing

Reverse proxy for backend API

Cache control to always load the latest build

2. API URL updated in Angular service

File: frontend/src/app/services/tutorial.service.ts

Changed from:

const baseUrl = 'http://localhost:8080/api/tutorials';


To:

const baseUrl = '/api/tutorials';


This makes all API requests go through Nginx inside the container.

3. Frontend Dockerfile

Created to:

Build Angular

Serve using Nginx

B. Backend Changes
1. Backend Dockerfile added

Used to containerize the Express server.

2. MongoDB URL updated

File: backend/app/config/db.config.js

Updated from:

mongodb://localhost:27017/dd_db


To:

mongodb://mongo:27017/tutorial_db


This connects backend with the MongoDB container.

C. Root Folder Additions
1. docker-compose.yml

Contains three services:

mongo

backend

frontend (includes Nginx inside its image)

A separate Nginx service is not required.

2. GitHub Actions workflow

File: .github/workflows/deploy.yml

It performs:

Build backend image

Build frontend image with --no-cache

Push both to DockerHub

SSH into EC2

Pull latest GitHub code

Pull latest Docker images

Restart docker-compose

Complete auto-deployment

3. EC2 Setup Instructions

SSH into EC2 and run:

sudo apt update
sudo apt install docker.io -y
sudo apt install docker-compose -y
sudo usermod -aG docker $USER && newgrp docker
sudo systemctl enable docker
sudo systemctl start docker


The machine is now ready for deployment.

4. GitHub Secrets Required

Go to:

GitHub → Repo → Settings → Secrets → Actions


Add the following:

Secret	Description
DOCKER_USERNAME	DockerHub username
DOCKER_PASSWORD	DockerHub password/token
EC2_HOST	EC2 public IP
EC2_USER	ubuntu
EC2_SSH_KEY	Content of .pem SSH key

Note
Since this setup does not use an Elastic IP, after every EC2 stop/start, the public IP changes.
Update the EC2_HOST secret with the new IP every time.

5. Running This Project From Scratch
Step 1 — Launch an EC2 instance

Install Docker and Docker Compose using the commands above.

Step 2 — Fork or clone the repository
git clone https://github.com/YOUR_USERNAME/DISCOVER-DOLLAR-ASSIGNMENT.git

Step 3 — Push the project to your GitHub repository

Update your DockerHub username in:

docker-compose.yml

Step 4 — Configure GitHub Secrets

Add:

DOCKER_USERNAME

DOCKER_PASSWORD

EC2_HOST

EC2_USER

EC2_SSH_KEY

Step 5 — Push to the main branch

GitHub Actions will automatically:

Build Docker images

Push to DockerHub

SSH into EC2

Pull the latest code

Pull new images

Run docker-compose

Deploy the project

No manual deployment is needed.

6. Folder Structure
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

7. Testing After Deployment
Backend API
curl http://YOUR_EC2_PUBLIC_IP/api/tutorials

Frontend

Open in browser:

http://YOUR_EC2_PUBLIC_IP

8. Final Notes

This project demonstrates:

Complete Dockerization of MEAN stack

Nginx reverse proxy for backend API

Angular routing fixes with try_files

Fully automated CI/CD with GitHub Actions

Automatic deployment on EC2 with zero manual steps

Updated builds using no-cache strategy

Fresh frontend build served through Nginx

The system is clean, simple, and ready to extend.