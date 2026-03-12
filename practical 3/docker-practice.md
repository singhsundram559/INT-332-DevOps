# Day 03 - Docker Services: Apache + MySQL

## Objective

- Run Apache (httpd) container
- Configure port mapping
- Attach named volume for persistent web files
- Set environment variables
- Run MySQL 8 container
- Configure database via environment variables
- Debug using logs
- Verify database inside container
- Understand container lifecycle

---

# 🔹 Part 1 - Apache (httpd)

## Create Volume

```bash
docker volume create portaldata
Run Apache Container
docker run -d --name college_portal \
-p 9090:80 \
-e ENV=production \
-v portaldata:/usr/local/apache2/htdocs \
httpd
Verify Running
docker ps

Access in browser:

http://localhost:9090

Modify Web Content
docker exec -it college_portal bash
echo "<h1>College Portal - Production Mode</h1>" > /usr/local/apache2/htdocs/index.html
exit

Refresh browser to verify changes.

Debug Using Logs
docker logs college_portal
🔹 Part 2 - MySQL 8
Run MySQL Container
docker run -d --name mysql_server \
-p 3307:3306 \
-e MYSQL_ROOT_PASSWORD=rootpass \
-e MYSQL_DATABASE=college_db \
-e MYSQL_USER=college_user \
-e MYSQL_PASSWORD=college_pass \
mysql:8
Check Logs
docker logs mysql_server

Look for:
"ready for connections"

Enter Container
docker exec -it mysql_server bash
mysql -u root -p

Password:
rootpass

Verify Database
SHOW DATABASES;
USE college_db;
CREATE TABLE test (id INT);
SHOW TABLES;
Stop & Remove Containers
docker stop college_portal mysql_server
docker rm college_portal mysql_server
Key Learnings

Containers depend on main process lifecycle.

-p host:container maps ports.

Named volumes persist data.

Environment variables configure services.

docker logs is essential for debugging.

MySQL requires mandatory root password.

Services can run independently in separate containers.

Conceptual Architecture

Browser → Apache Container → Volume (portaldata)

Application → MySQL Container → Internal Database Storage


Save and close.

---
