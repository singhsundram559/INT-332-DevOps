📄 Day05 Documentation Content
# Day 05 - Docker Networking and Multi-Container Architecture

## Objective
Learn how containers communicate using Docker networks and how to run multiple services together.

---

## Step 1: Create Custom Docker Network


docker network create app_bridge


This creates a bridge network where containers can communicate.

Check networks:


docker network ls


---

## Step 2: Create Persistent Volume for Database


docker volume create mysql_data


Inspect volume:


docker volume inspect mysql_data


This volume stores MySQL data so it survives container removal.

---

## Step 3: Run MySQL Database Container


docker run -d --name mysql_db
--network app_bridge
-e MYSQL_ROOT_PASSWORD=root
-e MYSQL_DATABASE=myapp
-v mysql_data:/var/lib/mysql
mysql:8


Explanation:

- `--network app_bridge` connects container to custom network
- `MYSQL_ROOT_PASSWORD` sets database root password
- `MYSQL_DATABASE` creates database automatically
- `-v mysql_data:/var/lib/mysql` stores database files in volume

---

## Step 4: Run Backend Container


docker run -d --name backend_app
--network app_bridge
-e DB_HOST=mysql_db
node:18-alpine
sh -c "apk add curl && sleep 300"


Explanation:

- Node container simulates backend service
- `DB_HOST=mysql_db` allows backend to connect to database
- `sleep 300` keeps container alive for testing

---

## Step 5: Test Container Communication

Enter backend container:


docker exec -it backend_app sh


Test connectivity:


ping mysql_db


Result shows successful communication between containers.

---

## Step 6: Run Frontend Container


docker run -d --name frontend
--network app_bridge
-p 8080:80 nginx


Access application:


http://localhost:8080


---

## Architecture

      Docker Network (app_bridge)

 frontend (NGINX)
        |
        |
   backend_app (Node.js)
        |
        |
     mysql_db
        |
    mysql_data (volume)

---

## Key Learnings

- Docker networks allow containers to communicate.
- Containers can access each other using container names.
- Volumes store persistent data for databases.
- Multi-container architecture separates frontend, backend, and database services.
- This architecture is the foundation for Docker Compose and microservices.