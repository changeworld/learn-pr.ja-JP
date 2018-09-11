In the last unit, you worked with pre-created container images to perform some basic Docker operations. In this unit, you will create custom container images, push these images to a public container registry, and run containers from these images.

Container images can be created by hand or using what's called a Dockerfile to automate the process. The preferred method is using a Dockerfile, but this unit will demonstrate both methods. Understanding the manual process will help you better understand what's occurring when using a Dockerfile for automation.

## Manual image creation

When manually creating a container image, the following actions are taken:

- Start an instance of a container.
- Establish a terminal session with the container.
- Modify the container by installing software and making configuration changes.
- Capturing the container into a new image using the `docker capture` command.

In this first example, you start an instance of a container that's running Python, create a 'Hello World' application, and then capture the container to a new image.

First, run a container from the NGINX image. This command looks a bit different from the commands that you ran in the previous unit. Because you want to establish a terminal session with the running container, the `-t` and `-i` arguments are provided. Together, these arguments instruct Docker to allocate a pseudo terminal that will remain in a runnings state. In other words, the `-t` and `-i` arguments create an interactive session with the running container.

You then specify that the `python` container image is used, and the process to run inside of the container is `bash`.

```bash
docker run --name python-demo -ti python bash
```

After the command is run, your terminal session should switch to the containers pseudo terminal. This can be seen by the terminal prompt, which should have changed to something similar to the following:

```output
root@d8ccada9c61e:/#
```

At this point, you're working inside the container. You should find that working inside a container is much like working inside a virtual or physical system. For instance, you can list, create, and delete files, install software, and make configuration changes. For this simple example, a Python-based 'Hello World' script is created. This can be done with the following command:

```bash
echo 'print("Hello World!")' > hello.py
```

To test the script while you're still in the container, run the following command:

```bash
python hello.py
```

This will produce the following output:

```output
Hello World!
```

When you're satisfied that the script functions as expected, exit out of the container by typing `exit`:

```bash
exit
```

Back in the terminal of your local system, use the `docker ps` command to list all running containers:

```bash
docker ps
```

Notice that nothing is running. When you entered `exit` in the running container, the Bash process completed, which then stopped the container. This is the expected behavior and is ok.

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Use `docker ps` again and include the `-a` argument. This command will return a list of all containers, regardless if they're running.

```bash
docker ps -a
```

Notice that a container with the name *python-demo* has a status of *Exited*. This container is the stopped instance of the container that you just exited from.

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

To create a new container image from this container, use the `docker commit` command. The following example instructs *docker commit* to create an image named *python-custom* from the *python-demo* containers.

```bash
docker commit python-demo python-custom
```

After the command completes, you should see output similar to the following:

```output
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

Now run `docker images` to see a list of container images:

```bash
docker images
```

You should now see the custom Python image.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

Run a container from the new image. You also need to specify what command or process to run inside of the container. With this example, run `python hello.py`.


```bash
docker run python-custom python hello.py
```

The container will start and output the 'Hello World' message. The Python process is then complete and the container stops.

```output
Hello World!
```

## Automated image creation

In the last section, a container image was manually created. While this method works great for one-off or experiential image creation, it's not sustainable in a production environment. Container image creation can be automated using a *Dockerfile* and the `docker build` command. The *docker build* command essentially starts a container, runs the instructions found in the *Dockerfile*, and then captures the results to a new container image.

Create a file named *Dockerfile* and enter the following text:

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

The *FROM* statement specifies the base image to be used during image creations. The *RUN* statement runs commands inside of the container. *RUN* can be used to install software, make configuration changes, and cleanup the container before the capture event. The *CMD* statement specifies the process that should run when a container is started.

Use the `docker build` command to create a new container image using the instructions specified in the Dockerfile.

```bash
docker build -t python-dockerfile .
```

You should see output similar to the following.

```output
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM python
 ---> 638817465c7d
Step 2/4 : WORKDIR ./app
 ---> Running in 990d17e86466
Removing intermediate container 990d17e86466
 ---> 59a074a092cc
Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
 ---> Running in aed707c53bc5
Removing intermediate container aed707c53bc5
 ---> d7f55a9d0e85
Step 4/4 : CMD python hello.py
 ---> Running in e87ec55a8d36
Removing intermediate container e87ec55a8d36
 ---> 98c39b91770f
Successfully built 98c39b91770f
Successfully tagged python-dockerfile:latest
```

Use the `docker images` command to return a list of container images.

```bash
docker images
```

You should now see the custom image.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

Use the `docker run` command to run a container from the custom image.

Notice here that no arguments have been provided to the `docker run` command. Unlike when you manually create a container image, a Dockerfile allows you to include a command to run when the container starts. In this case, the specified command is `python hello.py`, which causes the container to run the Python script.

```bash
docker run python-dockerfile
```

After you run the command, you should see the container output.

```output
Hello World!
```

## Push the image to a public registry

Docker Hub is a public container registry. In order to push container images to Docker Hub, you must have a Docker account. If needed, visit the [Docker Hub site](https://hub.docker.com/) to register for an account.

After you have a Docker Hub account, the container image must be tagged with the account name. To do so, use the `docker tag` command.

In the following example, the *python-dockerfile* image is tagged with a Docker Hub account name. Replace `<account name>` with your Docker Hub account name.

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

Next, make sure that you are logged into Docker Hub using the `docker login` command.

```bash
docker login
```

Finally push the *python-dockerfile* image to Docker Hub using the `docker push` command. Replace `<account name>` with your Docker Hub account name.

```bash
docker push <account name>/python-dockerfile
```

While the container image is being uploaded to Docker Hub, you will see output similar to the following:

```output
The push refers to repository [docker.io/account/python-dockerfile]
f39073ca4d5a: Pushed
9dfcec2738a9: Pushed
ffab8273c674: Mounted from account/python
e6f6cbe5e14e: Mounted from account/python
3eb255f8a6ac: Mounted from account/python
b860b6c48eec: Mounted from account/python
1fa8778eb779: Mounted from account/python
fa0c3f992cbd: Mounted from account/python
ce6466f43b11: Mounted from account/python
719d45669b35: Mounted from account/python
3b10514a95be: Mounted from account/python
```

The container image is now stored in Docker Hub and can be accessed from any Internet-connected machine using `docker pull` or `docker run`.

## Summary

In this unit, you created two container images. The first was created manually and the second was automated using a Dockerfile.
