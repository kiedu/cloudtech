# DOCKER (practical tasks) 

Source: https://campus.epam.ua/ua/training/3619

## Environment preparation

All practical tasks are designed to be ran within a *virtual machine* (VM), specially pre-built for this course. The steps following describe how to install and configure the VM first time (all subsequent tasks should be completed on the same VM):

1. Download and install **Oracle VirtualBox 7**. Use the appropriate version for your OS. The same page contains a link to **Oracle VM VirtualBox Extension Pack**, which should be downloaded and installed as well.

2. Make sure that your VirtualBox installation has at least one host-only network configured.

3. Download an image of **pre-built VM**.

4. Double-click on the downloaded image. A new VirtualBox window will appear, asking for import parameters. Please, pay your attention on the **Machine Base Folder** parameters: the parameter specifies destination folder to store imported VM's files. The folder should be located on a drive with at least **3 Gb** of free space. Up to **20 Gb** might be required during the course completion. All other parameters might be left as they are. Click **Finish** to import the VM.

5. When import is finished, open VM's settings and go to **Networking**. Make sure that **Adapter 1** is attached to **NAT** network, and **Adapter 2** - to **Host-only network**, created on the the step 2.

6. Start the VM. As soon as it finish boot, some login hints will be displayed. Login to proceed or to shut the VM down.


### Information for users of ARM-based Macs

Practical tasks for this course are designed to be completed within the attached VM. Since the VM is built around x86 version of Debian Linux, it will not run on ARM-based Macs. The alternative solution for such users is to use a cloud-based Debian Linux VM. All major cloud providers have a trial period for new users, which should be enough to complete this course. *Note:* this package should be installed on a such VM to add binaries/files required by the course.


## General recommendations

* The VM is shipped with a set of automated checks. It's advised to call the lesson-specific check after completion of all the practical tasks within a lesson. The check might be called unlimited number of times.

* Most of automated checks provide a secret word in case of successful task completion. Copy and paste the word in to appropriate input field under the task.

* Do not destroy created resources until the finish of all lesson's task - it may make negatively impact on verification process.


## Practical tasks

After completion of all the practical tasks for this lesson, run **checkup-basics** in the course VM to verify your results. The command will check your work and show some secret phrases. The phrases should be entered into the appropriate fields below tasks.


## Task 1

1. Install Docker Engine on your VM and check that it is installed successfully.
2. Check the version of Docker.
3. Execute **docker --help** and check that you see help for docker usage.
4. Сheck that docker service is up and running.
5. Add user **debian** to **docker group** to make sure that Docker commands might be executable without **sudo** command. *Note*: This may require to re-login into the VM.


## Task 2

1. Download **busybox** image from **Docker registry**.
2. Print a list of all images.
3. Run **busybox** container and see what happened, think why.
4. Run **busybox** container again and print hey from busybox container, using **echo 'hey from busybox container'** command.
5. Run **docker ps** command and see the output, think about this output.
6. Print a list of all containers.


## Task 3

1. Launch a new Docker container based on the **busybox** image with a name **busybox-container-1**.
2. Print a list of all containers and compare the container names that were created in the previous tasks and in this one.
3. Launch a new Docker container based on the **busybox** image with a name **busybox-container-1** and print a message **"My named container"**, the name must be the same as in first step. *Conflict, right?* Remove the container with a name **busybox-container-1** and launch a new Docker container based on the **busybox** image with a name **busybox-container-1** and print a message **"My named container"** again.
4. Print a list of all containers. Pay your attention to commands field, it should be different.
5. Recreate the situation with the conflict, but now *don't delete* the second container. Rename it to **busybox-container-2** instead.
6. Print a list of all containers and compare names and commands. Names should be different, commands should be the same.


## Task 4

1. Launch a new container named **Interactive-container** based on **busybox** image in Interactive mode and execute **ls** command and **echo 'WoW! I am in a container!!!'** inside your container. *Note: Use sh command for busybox image.*
2. Launch a new container named **Remove-container** based on **busybox** image and execute command **echo 'container will be removed'** and remove it. All should be done in a one command. Check that you container dosn't exist.


## Task 5

1. Launch a new Docker container based on **docker/getting-started** image in *detached mode*, publish container's port **80** to host port **80**, *don't use --rm flag*. Check in your browser that you can see *Getting Started* tutorial for Docker page.
2. List all running containers and check that container from the previous step is **UP**.
3. Save the output of the next step to the file **/opt/docker/basics/task5/task5.txt**, output of each subtask or command should start from a new line:

```
"Id": "c61c9d913dc9882c4523ee7dd90917995ad99bce4f6c1c5c562b9dd1b6b9ec95"

"Pid": 859

"IPAddress": "172.17.0.2"

"MacAddress": "02:42:ac:11:00:02"
```

4. Get full **ID, PID, IPAddress, MacAddress** of a Docker container from the first step.
5. Stop Docker container created on the first step.


## Task 6

1. Check which kernel version is installed on your VM.
2. Launch a new Docker container based on **busybox** image and check which kernel version is used for container.
3. Save the output of steps 1 and 2 to the file **/opt/docker/basics/task6/task6.txt**, each on a new line.
4. Compare kernel versions from the VM and the container. How do you think, why the outputs look this way?


## Task 7

1. Launch 2 Docker containers based on **nginx:alpine** image in *detached mode*. Use an appropriate flag to remove container after stop. Containers should be named **SRV1** and **SRV2**.
2. Connect to **SRV1** container interactively and get *PIDs* of **nginx** processes using **ps** in the container.
3. Repeat the same actions for the container **SRV2**.
4. Get **PIDs** of nginx processes in your VM.
5. Compare master **PIDs** for **SRV1** and **SRV2**. If PIDs are equal please add word **EQUAL** to the file **/opt/docker/basics/task7/task7_1.txt**, otherwise add word DIFF to the same file.
6. Stop **SRV1** container, container should be removed after stop.
7. Launch **SRV1** container based on **nginx:alpine** image in *detached mode*. Use appropriate flags to remove the container after stop, and to run it in the same *PID* namespace as for container **SRV2**.
8. Connect to **SRV1** container interactively and get **PIDs** of nginx processes using **ps** in the container.
9. Compare master **PIDs** from the previous step. If **PIDs** are equal please add word **EQUAL** to the file **/opt/docker/basics/task7/task7_2.txt**, otherwise add word DIFF to the same file.





