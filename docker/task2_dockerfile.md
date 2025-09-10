# DOCKER (practical tasks) 

Source: https://campus.epam.ua/ua/training/3619

## Practical tasks

After completion of all the practical tasks for this lesson, run **checkup-dockerfile** in the course VM to verify your results. The command will check your work and show some secret phrases. The phrases should be entered into the appropriate fields below tasks.


## Task 1

1. Create your own Dockerfile based on **alpine:3.17** image and install **nginx** using the following command **apk add nginx=1.22.1-r1**.
2. Build it with **docker build** command with name **my_own_image**.
3. Inspect history of this image after the building.
4. Use command **docker tag** for this new image and give it a new name **my_new_image:1.0.0**. Compare these 2 images.
5. Run container from one of these images and check the version of installed *nginx* with **nginx -v**.


## Task 2

1. Create Dockerfile based on **alpine:3.17** and with **CMD ["echo", "Hello, EPAM!"]**, build the image from this Dockerfile with name **cmd_image**.
2. Launch a container from this image without any additional arguments. After that launch container with passing additional argument uname (example: **docker run cmd_image uname**). Compare the outputs.
3. Create a new Dockerfile based on **alpine:3.17** and with **ENTRYPOINT ["echo", "Hello, EPAM!"]**, build the image from this Dockerfile with name **entrypoint_image.**
4. Launch a container from this image without any additional arguments. After that launch container with passing additional argument and others! (Example: **docker run entrypoint_image and others!**). Compare the outputs.
5. Create a new Dockerfile based on **alpine:3.17** which contains both **ENTRYPOINT ["echo", "Hello, EPAM!"]** and **CMD ["echo", "Hello, EPAM!"]** directives, build the image from this Dockerfile with name **combine_image**.
6. Launch a container from this image without any additional arguments. After that launch container with passing additional argument world! (Example: **docker run combine_image world!**). Compare the outputs.


## Task 3

1. Create Dockerfile based on **alpine:3.17** image and using the directive **ADD** add the following archive **https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.39.0.tar.gz** into the **/home** directory in the image.
2. Build it with **docker build** command with name **add_url_image**. Run container from this image, check the content of the **/home** directory and notice if anything has been added there.
3. Do the same but using the command **COPY**. You will receive the following error during the build: **source can't be a URL for COPY**.
4. Download to your local host the archive with name **git-2.39.0.tar.gz**, which we used in the 1 step.
5. Change Dockerfile with **COPY** directive specifying path to the local archive instead of URL. Build it with **docker build** command with name **copy_local_image**. Run container from this image, check the content of the **/home** directory and notice if anything has been added there.
6. Change Dockerfile using the directive **ADD** instead of **COPY**. Build it with docker build command with name **add_local_image**. Run container from this image, check the content of the **/home** directory and notice if anything has been changed there.


## Task 4

### Sub-task 1

1. Run Docker container from **nginx:1.23.3** and check with command **pwd** which directory is used.
2. Create a Dockerfile based on **nginx:1.23.3** image and using the directive **WORKDIR** specify working directory **/my-own-directory**.
3. Build it with **docker build** command with name **dir_image**. Run container from this image and check with the command **pwd** the working directory.

### Sub-task 2

1. Add to the Dockerfile from the previous sub-task directive **EXPOSE** with value **8080** and **LABEL** with key **description** and value **my_custom_label**.
2. Build it with **docker build** command with name **expose_image**. Run container from this image *without* specifying additional argument **-p** and check if this container available by IP in web browser or not. After that run container from this image with specifying additional argument **-p 8080:8080** and check availability now.
3. Inspect image using command **docker inspect** and find specified in Dockerfile *label*.

### Sub-task 3

1. Add to the Dockerfile from the previous task directive **USER** with value **nginx**.
2. Build it with **docker build** command with name **user_image**. Run container from this image with the command **id** to see which user is used there.


## Task 5

1. Create a Dockerfile based on **alpine:3.17** image and using the directive **ENV** add environment variable **MY_ENV=world**. Also add the following line to the Dockerfile: **RUN echo Hello, $MY_ENV!**.
2. Build it with **docker build** command with name **env_image**. Check the output during the building. Run a container from this image and run the command **echo $MY_ENV** inside the container.
3. Change Dockerfile using the directive **ARG** instead of **ENV**. Build it with **docker build** command with name **arg_image**. Check the output during the building. Run a container from this image and run the command **echo $MY_ENV** inside the container.
4. Build it again with providing the special argument **--build-arg**, overriding the value of the variable to **MY_ENV=EPAM**, and give a name for this image **over_arg_image**. Check the output during the building. Run a container from this image and run the command **echo $MY_ENV** inside the container.
5. Create a new Dockerfile based on **alpine:3.17** image, using directive **ARG** add variable **CUSTOM_USER=$CUSTOM_USER** and add directive **USER** with value **${CUSTOM_USER}**.
6. Build it with **docker build** command with name **arg2_image** and passing special parameter **--build-arg CUSTOM_USER=100**. Run a container from this image with command **id**. Rebuild the image passing value **999** for **CUSTOM_USER** and check the result of **id** command after that.


## Task 6

1. Create a Dockerfile based on **alpine:3.17** image and using the directive **RUN** install the following packages: **htop, postgresql-client, vim, git, jq=1.6-r2**. Use separate **RUN** directive for installation of each package.
2. Build it with **docker build** command with name **opt_image**. Check the *history* of the resulted image. You have to find *5 separate layers*, created by **RUN** directive.
3. Change the Dockerfile for using only one directive **RUN** for installation all needed packages (Tips here).
4. Build it with **docker build** command with name **opt_image2**. Check the *history* of the resulted image. You must find only *1 separate layer*, created by **RUN** directive.


##  Task 7

1. Using the command **docker run** run the container in *detached mode* from image **registry:2** with name **registry**. Also publish container's port **5000** to the host's port **5000** and specify option **--restart=always**.
2. Using the command **docker tag** create a tag **localhost:5000/my-alpine** from **alpine:3.17**.
3. Push this **localhost:5000/my-alpine** image to the local registry using the command **docker push**.
4. Using the command **docker rmi** delete images **localhost:5000/my-alpine** and **alpine:3.17**.
5. Using the command **docker pull** download image **localhost:5000/my-alpine** from the *local registry*.


## Task 8

1. Create a **multi-stage** Dockerfile for the simple web Golang application located in **/opt/docker/source/dockerfile/main.go**. 

The first stage has to be based on **golang:1.19**. The main goal of the first stage is compiling of the golang application using the command **go build -o hello main.go** (Note: Use **CGO_ENABLED=0** environment variable for compilation). 

On the second stage you have to *copy* compiled binary of the application from the *first stage*. For getting the minimum size of the final image you have to use **FROM scratch** for the second stage. Final image has to have entrypoint to run the golang application. You can use as much as possible directives from the previous sections.

2. Build it with **docker build** command with name **multi_image**. Run container from this image in detached mode exposing port **8999**. Check in the browser or via the **curl/wget** the response of the endpoint **localhost:8999/hello**. Inspect which directives lead to creation of new layer and which not.
