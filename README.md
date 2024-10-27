# 🗄️ Distributed MongoDB with HAProxy Load Balancing using Docker

## 📋 Project Overview

This project sets up a **distributed MongoDB system** with **HAProxy** for load balancing, containerized with Docker. The setup ensures **high availability**, **fault tolerance**, and **scalability** by replicating data across multiple MongoDB instances and using HAProxy to balance requests across them. Ideal for applications needing reliable, scalable, and distributed database infrastructures.

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [Setup Instructions](#-setup-instructions)
- [Testing the Setup](#-testing-the-setup)
- [Features](#-features)
- [Future Enhancements](#-future-enhancements)
- [License](#-license)

## 🛠️ Technologies Used
- **🐳 Docker**: Containerizes MongoDB and HAProxy services.
- **📊 MongoDB Replica Set**: Ensures data replication across multiple nodes.
- **🔀 HAProxy**: Load balancer that distributes incoming requests among MongoDB instances.
- **🖥️ MongoDB Compass**: GUI tool for managing MongoDB instances and verifying data replication.

## 📂 Project Structure

```plaintext
distributed-mongo-haproxy/
├── data/
│   ├── mongo1/       # Persistent data storage for mongo1
│   ├── mongo2/       # Persistent data storage for mongo2
│   └── mongo3/       # Persistent data storage for mongo3
├── haproxy/
│   └── haproxy.cfg   # Configuration file for HAProxy
├── docker-compose.yml # Docker Compose file to set up MongoDB and HAProxy
└── mongo-init.js      # Script to initialize MongoDB replica set
```

## ⚙️ Setup Instructions

### 🔧 Prerequisites
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop) on your system.

### 📝 Steps

1. **Clone the Repository**  
   ```bash
   git clone https://github.com/yourusername/distributed-mongo-haproxy.git
   cd distributed-mongo-haproxy
   ```

2. **Set Up Docker Compose**  
   The `docker-compose.yml` file defines three MongoDB instances and HAProxy. Modify if necessary, then run:
   ```bash
   docker-compose up -d
   ```

3. **Initialize the MongoDB Replica Set**  
   Connect to the primary MongoDB instance in interactive mode:
   ```bash
   docker exec -it mongo1 mongosh -u root -p example --authenticationDatabase admin
   ```
   Initialize the replica set:
   ```javascript
   rs.initiate({
      _id: "mongo-replica-set",
      members: [
         { _id: 0, host: "mongo1:27017" },
         { _id: 1, host: "mongo2:27017" },
         { _id: 2, host: "mongo3:27017" }
      ]
   });
   ```

4. **Verify the Replica Set**  
   Use MongoDB Compass to connect to each instance and verify data replication across `mongo1`, `mongo2`, and `mongo3`.

5. **Load Balancing with HAProxy**  
   Connect to HAProxy on port `27020` to see load balancing in action:
   - **Hostname:** `localhost`
   - **Port:** `27020`

## 🧪 Testing the Setup

1. **Insert Test Data**  
   Insert data through MongoDB Compass or the Mongo shell connected to HAProxy at `localhost:27020`.
   
2. **Check Data Replication**  
   Verify data replication across all MongoDB instances (`mongo1`, `mongo2`, `mongo3`) using MongoDB Compass.

3. **Simulate Failover**  
   Stop the primary instance `mongo1` and observe as one of the secondary nodes is automatically elected as the new primary. This ensures continuity and availability.

## 🌟 Features

- **High Availability**: Database remains operational even if one instance fails.
- **Load Balancing**: HAProxy distributes incoming requests across multiple MongoDB instances.
- **Data Replication**: MongoDB replica set ensures data consistency across all nodes.
- **Scalability**: Easily add more MongoDB nodes to handle increased load.

## 🚀 Future Enhancements

- **📈 Monitoring and Alerts**: Integrate monitoring tools like **Prometheus** and **Grafana**.
- **🔒 Security**: Add SSL/TLS encryption and refine access control settings.
- **💾 Automated Backups**: Schedule automated backups for enhanced data protection.

## 📜 License

This project is licensed under the MIT License.
