# EpicBook Application Deployment on AWS 🚀


## Project Overview

This project demonstrates a production-style deployment of a Node.js application on AWS infrastructure.
The application backend runs on EC2, the database is hosted on Amazon RDS (MySQL), and Nginx acts as a reverse proxy while PM2 manages the Node.js process.

The goal of this project is to simulate a real-world DevOps deployment workflow including infrastructure setup, database configuration, application deployment, and troubleshooting.

### Architecture
User Browser
      │
      ▼
Nginx (Port 80)
      │
      ▼
Node.js Backend (Port 3000 - Managed by PM2)
      │
      ▼
AWS RDS MySQL Database


### Technologies Used

1. AWS EC2 (Ubuntu)

2. AWS RDS (MySQL)

3. Node.js

4. Express.js

5. Nginx (Reverse Proxy)

6. PM2 (Process Manager)

7. Linux (Ubuntu)

8. Git & GitHub

### Infrastructure Setup

#### Create EC2 Instance

Launch Ubuntu EC2 instance and connect via SSH.

ssh -i key.pem ubuntu@EC2_PUBLIC_IP
Install Required Packages
sudo apt update
sudo apt install nodejs npm -y
sudo apt install nginx -y
sudo apt install mysql-client -y
Database Setup (AWS RDS)

Connect to the database:

mysql -h RDS_ENDPOINT -u admin -p

#### Create database:

CREATE DATABASE bookstore;

Verify:

SHOW DATABASES;
Import Database Schema

Navigate to SQL files:

cd ~/theepicbook/db

#### Import schema:

mysql -h RDS_ENDPOINT -u admin -p bookstore < BuyTheBook_Schema.sql

Import seed data:

mysql -h RDS_ENDPOINT -u admin -p bookstore < author_seed.sql
mysql -h RDS_ENDPOINT -u admin -p bookstore < books_seed.sql

#### Verify tables:

mysql -h RDS_ENDPOINT -u admin -p -e "USE bookstore; SHOW TABLES;"
Deploy Backend Application

#### Clone repository:

git clone https://github.com/pravinmishraaws/theepicbook.git
cd theepicbook

#### Install dependencies:

npm install

#### Start application:

npm start

Verify backend running:

ss -tulpn | grep 3000

#### Test backend response:

curl http://localhost:3000
Configure Nginx Reverse Proxy

#### Create Nginx configuration:

sudo nano /etc/nginx/sites-available/epicbook

Configuration:

server {
    listen 80;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:3000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

#### Enable configuration:

sudo ln -s /etc/nginx/sites-available/epicbook /etc/nginx/sites-enabled/

Test configuration:

sudo nginx -t

Restart Nginx:

sudo systemctl reload nginx

#### Setup PM2 (Production Process Manager)

Install PM2:

npm install -g pm2

Start application:

pm2 start server.js

View running processes:

pm2 list

#### Enable auto restart on reboot:

pm2 startup
pm2 save

### Deployment Verification

#### Check backend port:

ss -tulpn | grep 3000

#### Test website:

curl http://EC2_PUBLIC_IP

#### Test API endpoint:

curl http://EC2_PUBLIC_IP/api/

#### Check database connectivity:

mysql -h RDS_ENDPOINT -u admin -p -e "SELECT 1;"

### Issues Faced and Solutions


#### Database Connection Error

Error:

 ECONNREFUSED 127.0.0.1:3306

Cause:
Application attempted to connect to local MySQL instead of RDS.

Solution:
Updated Sequelize configuration to use the RDS endpoint.

#### Database Not Found

Error:

Unknown database 'bookstore'

Cause:
Database was not created before importing schema.

Solution:

CREATE DATABASE bookstore;

#### Error:
502 Bad Gateway (Nginx)

Cause:
Nginx could not reach Node backend.

Solution:
Restart backend service and verify port 3000 was listening.

#### Error:
Node Application Stopping After SSH Disconnect

Cause:
Application was started with npm start.

Solution:
Used PM2 to manage Node.js process.

## Key DevOps Concepts Demonstrated

1. Cloud Infrastructure Deployment

2. Database Management with AWS RDS

3. Reverse Proxy Configuration

4. Production Process Management

5. Linux Server Administration

6. Troubleshooting Deployment Issues


# Final Result

The EpicBook application was successfully deployed using a production-style cloud architecture with EC2, RDS, Nginx, and PM2.

The application is accessible through the public EC2 IP address via Nginx reverse proxy.

# Mentor
A huge thanks to my mentor Pravin Mishra. 

# Author
Nirmalya 
DevOps Enthusiast | Cloud & Infrastructure Projects