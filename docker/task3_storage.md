# DOCKER (practical tasks) 

Source: https://campus.epam.ua/ua/training/3619

## Practical tasks
After completion of all the practical tasks for this lesson, run **checkup-storage** in the course VM to verify your results. The command will check your work and show some secret phrases. The phrases should be entered into the appropriate fields below tasks.

## Task 1
1. Run a container in detached mode from **alpine:3.17** image and using the argument **-v [host-src]:[container-dest]** bind mount the following directory **/opt/docker/storage/task1** from your host machine into the directory **/home** inside the container.
2. Using command **docker exec**, run shell command for this container, go to the directory **/home** inside the container and create 2 files: **myfile1** containing the string **Hello world!**, and **myfile2** containing the string **Hello from EPAM!**.
3. Exit from the container and check that these 2 files (which you created inside the container) also exist in the directory **/opt/docker/storage/task1**. Delete the file myfile1, go again into the container and check that file doesn't exist there too (myfile2 should still exist).
4. Also check with **docker volume ls** command if there were created any volumes or not.


## Task 2
1. Run a container in detached mode with name **mypostgres** from **postgres:15.1** image, specifying only one additional environment variable **POSTGRES_PASSWORD=mysecretpassword**.
2. Using the command **docker inspect**, find the name of volume, which was created automatically for your container.
3. Run command **docker volume ls** and find there the volume, which you found on the previous step.
4. Using the command **docker volume inspect**, find the mountpoint on your host system for this volume. Check this location and find PostgreSQL files there.
5. Run a container with name **myalpine** from **alpine:3.17** image and using the argument **--volumes-from mypostgres** share the volume created for postgresql container.
6. Using the **docker inspect**, check that this new container uses the same volume as postgresql container. Go inside the container and check the directory **/var/lib/postgresql/data/**.


## Task 3
1. Using the command **docker volume create** create a volume with name **my_volume**. Verify your volume with command **docker volume ls**.
2. Run PostgreSQL container from the previous task, but with name **mypostgres2** and additional argument **-v [host-src]:[container-dest]**, where **[host-src]** will be **my_volume volume**, **[container-dest]** will be **/var/lib/postgresql/data/**.
3. Using the command **docker volume inspect**, find the mountpoint on your host system for your volume. Check this location and find PostgreSQL files there.
4. Run container with name **myalpine2** from **alpine:3.17** image and using the argument **--volumes-from mypostgres2** share the volume created for postgresql container.
5. Using the **docker inspect**, check that this new container uses the same volume as postgresql container. Go inside the container and check the directory **/var/lib/postgresql/data/**.


## Task 4
1. Run a container in detached mode with name **tmpfs_container** from **nginx:1.23** image, specifying arguments **--mount type=tmpfs,destination=/my_folder1** and **-v /tmp:/my_folder2**.
2. Using the command **docker inspect**, find the configs for both these volumes and compare them.
3. Using command **docker exec**, run shell command for this container and using the utility **dd** create two 512 Mb files named **speed_file** in folders **/my_folder1** and **/my_folder2** and compare write speed for them. (Use the following command **dd if=/dev/zero of=/my_folder1/speed_file bs=512M count=1**). Output of dd command should be saved in a file named **result** in corresponding folder **my_folder1** and **my_folder2**.
4. Inside the container create a file **/my_folder1/my_file1** containing a string **Hello from my_file1!**. And also create a file **/my_folder2/my_file2** containing a string **Hello from my_file2!**.
5. Exit from the container and with commands **docker stop** and **docker rm** stop and delete this container. After that check the content of the file **/tmp/my_file2**.


## Task 5

*Sub-task 1*

1. Run a container in detached mode with name **backup_container** from **nginx:1.23** image, specifying an argument **-v /etc/nginx**.
2. Launch a new container from **alpine:3.17**, mount the volume from **backup_container** container, mount the host directory **/opt/docker/storage/task5** as **/backup** and pass a command which tars the content of the **/etc/nginx** to a **backup.tar** file inside **/backup** directory (**tar cvf /backup/backup.tar /etc/nginx**).
3. After the completion of the backup, check the file **/opt/docker/storage/task5/backup.tar** on your local host.

*Sub-task 2*

1. Run a new container in detached mode with name **restore_container** from **alpine:3.17**, specifying an argument **-v /etc/nginx** and passing the command sleep infinity.
2. Launch a new container from **alpine:3.17**, mount the volume from **restore_container** container, mount the host directory **/opt/docker/storage/task5** as **/backup** and pass a command which untars the content of the **/backup/backup.tar** file to **/etc/nginx directory** (**tar xvf /backup/backup.tar --strip 2 -C /etc/nginx**). Check the content of the **/etc/nginx** directory in the **restore_container** container.


## Task 6
1. Using the command **docker volume create** create a volume with name **sidecar_volume**.
2. Run a container in detached mode with name **producer** from **alpine:3.17** image with additional argument **-v [host-src]:[container-dest]** (where **[host-src]** will be **sidecar_volume** volume, **[container-dest]** will be **/home**) and also add the following command for this container: **sh -c 'while sleep 5; do echo "Hello from EPAM!" >> /home/logs ; done'**.
3. Using the command **docker exec** go inside the container and check, that every 5 seconds a new line **Hello from EPAM!** is being added to the file **/home/logs**.
4. Run a new container in detached mode with name **consumer** from **alpine:3.17** image with additional argument **-v [host-src]:[container-dest]** (where **[host-src]** will be **sidecar_volume** volume, **[container-dest]** will be **/opt**) and also add the following command for this container: **tail -f /opt/logs**. This container will print every 5 seconds a new message from volume, which produce producer container.
