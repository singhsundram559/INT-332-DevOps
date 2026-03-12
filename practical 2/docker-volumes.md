# Day 02 - Docker Volumes (Persistent Storage)

## Objective

Understand:
- Why container data disappears
- How named volumes work
- Difference between normal container storage and volume storage
- Why containers stop in detached mode

---

## 1️⃣ Create Named Volume

```bash
docker volume create vol1
Verify:

docker volume ls


Inspect:

docker volume inspect vol1


Important Output:

Mountpoint: /var/lib/docker/volumes/vol1/_data


This is where Docker stores volume data internally.

2️⃣ Run Container with Volume (Incorrect Attempt)
docker run -d -v vol1:/data --name volTest ubuntu


Container exited immediately.

Reason:
Ubuntu image does not run any long-running process by default.
When the main process exits, the container stops.

3️⃣ Correct Way to Keep Container Running
docker run -d -v vol1:/data --name volTest ubuntu sleep infinity


Now container stays alive.

Verify:

docker ps

4️⃣ Create File Inside Volume
docker exec -it volTest bash


Inside container:

echo "Persistent Volume Test" > /data/test.txt
exit

5️⃣ Remove Container
docker rm -f volTest

6️⃣ Recreate Container Using Same Volume
docker run -it -v vol1:/data ubuntu


Inside container:

cat /data/test.txt


File still exists.

This proves volume persistence.

Key Learnings

Container filesystem is temporary.

Volumes store data outside container lifecycle.

-d mode requires a running process.

Ubuntu container exits immediately unless a process is kept alive.

Named volumes are managed by Docker.

Concept Summary

Without volume:

Container → Data deleted when container removed

With volume:

Container → Volume → Data persists

Conclusion

Today I understood Docker volume lifecycle, persistence behavior, and container process dependency.