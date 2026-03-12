# Day 01 - Docker File Copy Operations (Host ↔ Container)

## Objective
Understand how to copy files:
- From Container → Host
- From Host → Container
- Understand container lifecycle behavior

---

## 1️⃣ Run Ubuntu Container

```bash
docker run -dit --name myubuntu ubuntu
Verify container:

docker ps

2️⃣ Create File Inside Container

Open bash inside container:

docker exec -it myubuntu /bin/bash


Inside container:

mkdir /data
echo "Hello Docker" > /data/test.txt
exit


File created at:

/data/test.txt

3️⃣ Copy File from Container → Host
docker cp myubuntu:/data/test.txt C:\dockerTest\


Verify on host:

dir


File test.txt should appear.

4️⃣ Copy File from Host → Container
docker cp C:\dockerTest\sample.txt myubuntu:/data/sample.txt


Verify inside container:

docker exec -it myubuntu ls /data
docker exec -it myubuntu cat /data/sample.txt

Key Learnings

docker cp must be run from the host machine

Colon : must directly follow container ID or name

Container must exist (running or stopped)

Containers stop when main process exits

docker exec is preferred over docker attach

Files inside container are lost if container is removed

Common Mistakes Faced

❌ Running docker cp inside container
❌ Forgetting space in ls /data
❌ Using ls in Windows CMD (should use dir)
❌ Container not running while executing docker exec

Commands Summary
Action	Command
Start container	docker start myubuntu
Stop container	docker stop myubuntu
Remove container	docker rm myubuntu
Copy container → host	docker cp myubuntu:/path host_path
Copy host → container	docker cp host_path myubuntu:/path
Conclusion

Today I understood:

Host vs Container environment separation

Container lifecycle

File transfer between host and container

Correct usage of docker exec vs docker attach

This builds foundation for Docker volumes and persistent storage.
