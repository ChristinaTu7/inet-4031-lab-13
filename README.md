# Docker Lab: Containerizing a Three-Tier Application
**INET 4031 - Introductions to Systems**

This lab introduces Docker and Docker Compose by having you containerize a
real, multi-service application. You will package three components: Apache,
Flask, and MariaDB. These will be packaged into separate containers and wired together so they function as a complete application.

---

# Project Overview

This application is a ticket managing system that allows users to view and submit IT support tickets. The system is designed using a three-tier architecture:

- a frontend where users interact with the system
- a backend API that processes all of the requests
- a database that stores all of the ticket data

Users interact with the application through a web browser. This browser displays other existing tickets and allows users to create new ones as well. When a request is made, it is sent to the backend API, which processes the request and interacts with the database. The database then stores all ticket information and ensures that the data persists even if containers restart.

This lab focuses on understanding how these components are separated and containerized, yet connected through the use of Docker Compose.

# Prerequisites

Before running this application, the following components must be installed and configured onto the virtual machine:

- Docker
- Docker Compose
- Git
- Linux (Ubuntu recommended)

To verify any installation, run:

"docker --version
docker compose version"

If docker isn't installed or running, then the containers won't start.

# Getting Started

1. Clone the repository
"git clone <repository-url>
cd inet4031-testlab12"

2. Start the containers
"docker compose up -d"

3. Verify that all services are running
"docker compose ps"

You should see three services that are 'up' and reported as 'healthy'
- web
- app
- db


# Configuration

The application uses a .env file to define environment variables used by the containers. These variables include:

- database name
- database username
- database password
- database host

The .env file allows configuration without modifying the application code. If any values are incorrect or missing, the backend may fail to connect causing the health check to fail.

# Verification

To confirm that the application is working properly, you can do these following checks:

1. container status
"docker compose ps"

All containers should be running and healthy (web, app, db)

2. frontend response
"curl http://localhost:80/:

This expected result will be an HTML output

3. health
"curl http://localhost:80/health"

The results will show 
{"database":"connected","status":"healthy"}

This confirms that the API is running and successfully connected to the database

4. retrieve tickets
"curl http://localhost:80/api/tickets"

This will show a JSON array containing the ticket objects

5. create a ticket
"curl -X POST http://localhost:80/api/tickets \
  -H "Content-Type: application/json" \
  -d '{"title": "My first ticket", "description": "Testing the API"}'"

This will give you the result:
{"id": <number>, "message": "Ticket created successfully"}

6. persistence test
"docker compose stop db
docker compose start db"

This will confirm data persists

After waiting for the database to become healthy
"curl http://localhost:80/api/tickets"

This will confirm that the database is using persistent storage

7. check script
"./check.sh"

A successful run will meet all the required conditions, including the service health, API functionality, and persistence.


# Lab 13 - Kubernetes Deployment
In lab 13, the application was moved from a Docker Compose environment to Kubernetes. Instead of using Docker Compose to manage containers, the application now uses Kubernetes resources like deployment, services, and secrets. This allows the application to be managed in a more scalable and reliable way while also introducing self-healing features if a container fails.

# Deploy the Application
Use the command below to deploy all Kubernetes resources from the k8s directory:

'kubectl apply -f k8s/'

# Access the Dashboard
Once all pods and services are running, access the dashboard in a browser using the address below:

'http://(VM-IP):30080'
replace (VM-IP) with the IP address of your virtual machine
