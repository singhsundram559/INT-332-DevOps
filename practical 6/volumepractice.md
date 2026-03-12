Practical: Demonstrating Docker Volume Persistence

Objective



To create a Docker volume, attach it to a container, store data inside the volume, remove the container, and verify that the data persists when a new container is created using the same volume.



Step 1: Create Docker Volume



Command:



docker volume create projectdata

Explanation



docker volume create → creates a new persistent storage volume



projectdata → name of the volume



Verify Volume Creation



Command:



docker volume ls



Expected Output:



DRIVER    VOLUME NAME

local     projectdata



This confirms the volume is created successfully.



Step 2: Inspect the Volume



Command:



docker volume inspect projectdata



Expected Output (simplified):



\[

&nbsp;{

&nbsp;  "Name": "projectdata",

&nbsp;  "Driver": "local",

&nbsp;  "Mountpoint": "/var/lib/docker/volumes/projectdata/\_data"

&nbsp;}

]

Important Field

Field	Meaning

Mountpoint	Location where Docker stores the volume on the host



Example:



/var/lib/docker/volumes/projectdata/\_data

Step 3: Run Ubuntu Container with Volume



Command:



docker run -dit --name project\_container -v projectdata:/app/data ubuntu

Explanation

Option	Meaning

-d	Run container in background

-i	Interactive

-t	Allocate terminal

--name project\_container	Container name

-v projectdata:/app/data	Mount volume inside container

ubuntu	Image

Step 4: Enter the Container



Command:



docker exec -it project\_container bash

Step 5: Create File Inside Volume



Navigate to mounted directory:



cd /app/data



Create the file:



echo "This is my first volume." > report.txt



Verify file:



cat report.txt



Expected Output:



This is my first volume.



Exit container:



exit

Step 6: Stop and Remove Container



Stop container:



docker stop project\_container



Remove container:



docker rm project\_container



Note:

The container is deleted but the volume still exists.



Step 7: Run New Container Using Same Volume



Command:



docker run -dit --name project\_container\_new -v projectdata:/app/data ubuntu

Step 8: Access New Container



Command:



docker exec -it project\_container\_new bash



Navigate to directory:



cd /app/data

Step 9: Verify File Persistence



List files:



ls



Expected Output:



report.txt



Check file content:



cat report.txt



Output:



This is my first volume.



This confirms that Docker volume preserved the data even after container deletion.



Complete Command Flow (Write in Exam)

docker volume create projectdata



docker volume ls



docker volume inspect projectdata



docker run -dit --name project\_container -v projectdata:/app/data ubuntu



docker exec -it project\_container bash



cd /app/data



echo "This is my first volume." > report.txt



exit



docker stop project\_container



docker rm project\_container



docker run -dit --name project\_container\_new -v projectdata:/app/data ubuntu



docker exec -it project\_container\_new bash



cd /app/data



ls



cat report.txt

