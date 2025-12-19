🚀 CoinSync – Deployment on AWS EC2 using Docker

This document provides a step-by-step guide to deploy the CoinSync static web application (HTML, CSS, JavaScript) on an AWS EC2 Ubuntu instance using Docker and Nginx.

📌 Tech Stack

-- Frontend: HTML, CSS, JavaScript

-- Web Server: Nginx

-- Containerization: Docker

-- Cloud Platform: AWS EC2 (Ubuntu 22.04)

🏗️ Architecture Overview

      User Browser
           ↓
      EC2 Public IP
           ↓
      Docker Container
           ↓
      Nginx
           ↓
      HTML / CSS / JS

🔐 Prerequisites

-- AWS Account

-- EC2 Ubuntu Instance (22.04)

-- GitHub repository access

-- EC2 Security Group:

      SSH (22)
      
      HTTP (80)

⚙️ Step 1: Launch EC2 Instance

      Launch an Ubuntu 22.04 LTS EC2 instance
      
      Instance type: t2.micro (Free Tier)
      
      Attach a key pair (.pem)
      
      Configure Security Group:
      
      SSH   22   My IP
      HTTP  80   Anywhere

🔑 Step 2: Connect to EC2

      chmod 400 key.pem
      ssh -i key.pem ubuntu@<EC2_PUBLIC_IP>

🔄 Step 3: Update System

      sudo apt update -y && sudo apt upgrade -y

🐳 Step 4: Install Docker

      sudo apt install docker.io -y

Enable Docker:

      sudo systemctl start docker
      sudo systemctl enable docker

Fix permissions:

    sudo usermod -aG docker ubuntu && newgrp docker


Verify:

      docker --version

📦 Step 5: Install Git

      sudo apt install git -y

📥 Step 6: Clone Repository

      git clone https://github.com/PriyanshuSingh10114/CoinSync.git
      
      cd CoinSync

📝 Step 7: Prepare Project

Ensure the main HTML file is named:

index.html


(Nginx serves index.html by default)

🐋 Step 8: Create Dockerfile

Create a file named Dockerfile in the project root:

      FROM nginx:alpine
      
      RUN rm -rf /usr/share/nginx/html/*
      
      COPY . /usr/share/nginx/html
      
      EXPOSE 80
      
      CMD ["nginx", "-g", "daemon off;"]

🚫 Step 9: (Optional) Create

      .dockerignore
      .git
      .gitignore
      README.md
      node_modules

🧱 Step 10: Build Docker Image

      docker build -t coinsync-app .

▶️ Step 11: Run Docker Container

      docker run -d \
        --restart always \
        -p 80:80 \
        --name coinsync \
        coinsync-app


Verify:

      docker ps

🌐 Step 12: Access Application

Open your browser and visit:

      http://<EC2_PUBLIC_IP>


🎉 CoinSync is now live!

🔁 (Optional) Using Docker Compose

      docker-compose.yml
      version: "3.8"
      
      services:
        coinsync:
          build: .
          container_name: coinsync
          ports:
            - "80:80"
          restart: always


Run:

      docker-compose up -d --build

🧪 Debugging Commands

      docker ps
      docker logs coinsync
      docker exec -it coinsync ls /usr/share/nginx/html
