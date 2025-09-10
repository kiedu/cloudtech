# DOCKER (practical tasks) 

Source: https://campus.epam.ua/ua/training/3619

## Practical tasks

After completion of all the practical tasks for this lesson, run **checkup-networking** in the course VM to verify your results. The command will check your work and show some secret phrases. The phrases should be entered into the appropriate fields below tasks.

## Task 1

1. Run **busybox:1.36** container named **busy-task1** in detached mode with the **sleep infinity** argument passed during container startup (*Note: do the same for all busybox container in the further tasks*) and inspect which network is attached by default. Save driver's name to file **/opt/docker/networking/task1**. View existing networks and pay attention to DRIVER column.
2. Create a bridge network named **bridge-task1**.
3. Disconnect default network from the created container. Inspect that there is no network in container, try to ping 8.8.8.8 from container and check the output. Connect container to **bridge-task1** network and try again to ping 8.8.8.8 from the container and check the output.
4. Stop and delete **busy-task1** container. Delete created network named **bridge-task1**.


## Task 2

1. Create a bridge network named **bridge-task2**.
2. Run a container based on **nginx:1.23-alpine** and named **nginx-int-task2** in the created bridge network **bridge-task2**.
3. Try to get Nginx start page from host using **curl localhost**. Think about why you get such a result and try to get Nginx start page *using container ip*.
4. Run another container based on **nginx:1.23-alpine** and named **nginx-ext-task2** in the **host** network.
4. Try to get Nginx start page from host using **curl localhost**.
5. Inspect and compare NetworkSettings section in the created containers.


## Task 3

1. Run **busybox:1.36**-based container named **busy-none-task3** in **none** network.
2. Check the container’s network stack, by executing **ifconfig** command within the container. Notice that no **eth0** was created.
3. Do the same operations for **bridge** and **host** networks (container names **busy-bridge-task3** and **busy-host-task3** accordingly). Compare the results. Save the number of interfaces for each network (use ifconfig) to the file **/opt/docker/networking/task3** in the following format:
```
None - number
Bridge - number
Host - number
```


## Task 4

1. Create a bridge network named **bridge-task4**.
2. Run a **busybox:1.36**-based containers named **busy-task4** and **nginx:1.23-alpine** container named **nginx-task4** in the created network.
3. Check connection between containers using **wget -O - nginx-task4** from **busy-task4** to **nginx-task4** and save the output to **/opt/docker/networking/task4**.


## Task 5

1. Create 2 bridge networks named **bridge1-task5** and **bridge2-task5**.
2. Run a **busybox:1.36** container **busy-task5** in network **bridge1-task5**, a **redis:7.0** container **redis-task5** in network **bridge2-task5** and **nginx:1.23-alpine** container **nginx-task5** in both of networks.
3. Check connection between created containers:

* Go inside the Nginx container. Run **apk add redis** to interact with the Redis container. Try to connect to Redis container using **redis-cli -h redis-task5 -p 6379** and create a key-value pair using **SET docker lab** command.

* Go inside the Redis container. Check if data has appear using **redis-cli** command and then **GET docker**.

* Use the following command **nc -v -w 1 redis-task5 6379** from Busybox container to Redis container and also from Nginx container to Redis container. Save outputs to file **/opt/docker/networking/task5** in the following format:
```
Busy-Redis: output
Nginx-Redis: output
```

