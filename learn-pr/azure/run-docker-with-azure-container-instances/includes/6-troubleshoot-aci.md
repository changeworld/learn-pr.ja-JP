In this unit, you will perform some basic troubleshooting operations such as pulling container logs, container events, and attaching to a container instance. By the end of this module, you should understand basic capabilities for troubleshooting container instances.

## Create a container

Start by creating a container to use in this unit. If you still have the first container created in this module, skip this step:

```azurecli
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --ports 80 --ip-address Public
```

## Get logs from a container instance

To view logs from your application code within a container, you can use the `az container logs` command:

```azazurecli
az container logs --resource-group myResourceGroup --name mycontainer
```

The following is log output from the example container after the web app has been accessed a few times:

```bash
listening on port 80
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:24 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://23.101.136.193/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:25 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
::ffff:0.0.0.0 - - [20/Aug/2018:21:44:27 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36"
```

## Get container events

The `az container attach` command provides diagnostic information during container startup. Once the container has started, it also streams STDOUT and STDERR to your local console:

```azazurecli
az container attach --resource-group myResourceGroup --name mycontainer
```

Example output:


```bash
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-08-20 21:43:26+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:32+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Created container
(count: 1) (last timestamp: 2018-08-20 21:43:33+00:00) Started container

Start streaming logs:
listening on port 80
listening on port 80
```

## Execute a command in a container

Azure Container Instances supports executing a command in a running container. Running a command in a container you've already started is especially helpful during application development and troubleshooting. The most common use of this feature is to launch an interactive shell, so that you can debug issues in a running container.

This example starts an interactive terminal session with the running container:

```azurecli
az container exec --resource-group myResourceGroup --name mycontainer --exec-command /bin/sh
```

Once the command has completed, you are effectively working inside of the container. In this example, the `ls` command was run to display the contents of the working directory:

```bash
usr/src/app # ls
index.html         node_modules       package.json
index.js           package-lock.json
```

Enter `exit` to stop the remote session.

## Monitor container CPU and memory

You may want to pull metrics on CPU and memory usage. To do so, first get the ID of the Azure container instance. In this example, the ID is placed in a variable named `CONTAINER_ID`:

```azurecli
CONTAINER_ID=$(az container show --resource-group myResourceGroup --name mycontainer --query id --output tsv)
```

Now, use the `az monitor metrics list` command to pull back CPU usage information:

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric CPUUsage --output table
```

Example output:

```bash
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

The following command can be used to get memory usage information:

```azurecli
az monitor metrics list --resource $CONTAINER_ID --metric MemoryUsage --output table
```

Example output:

```bash
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

This information is also available in the Azure portal. To see graphical representation of CPU and memory usage information, visit the Azure portal overview page for a container instance.

![Azure portal view of Azure Container Instances CPU and memory usage information](../media-draft/cpu-memory.png)

## Clean up
<!---TODO: Update for sandbox?--->

This is the last unit of the Azure Container Instances learning module. At this point, you can cleanup the created resources by deleting the resource group. To do so, use the **az group delete** command:

```azurecli
az group delete --name myResourceGroup --no-wait
```

## Summary

In this unit, you learned about several troubleshooting operations such as pulling container logs, container events, and attaching to a container instance.