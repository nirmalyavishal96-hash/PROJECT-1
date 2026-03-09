# Architecture Diagram
![EpicBook Architecture](architecture/app-architecture.png)

# Architecture Explanation

Users access the application through the public EC2 IP address.

Nginx acts as a reverse proxy receiving requests on port 80.

Nginx forwards requests to the Node.js backend running on port 3000.

The Node application communicates with Amazon RDS MySQL located in a private subnet.

PM2 manages the Node.js process to ensure the application stays running.