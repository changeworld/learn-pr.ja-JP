Your company makes use of container images to manage compute workloads in the company. You use local Docker tooling to build your container images. The decision to make use of an Azure Container Registry now allows you to build container images in the cloud. 

You can now use Azure Container Registry Build to build these containers. Container Registry Build also allows for DevOps process integration with automated build on source code commit.

Let's automate the creation of a container image using Azure Container Registry Build.

## Create a container image with Azure Container Registry Build

You use a standard Dockerfile to provide build instructions. Azure Container Registry Build allows you to reuse any Dockerfile currently in your environment, including multi-staged builds.

We'll use new Dockerfile for our example. 

The first step is to create a new file named `Dockerfile`. You can use any text editor to edit the file. We'll use Visual Studio Code for this example.

```bash
code Dockerfile
```

Copy the following contents to your new Dockerfile. Make sure to safe the file. 

```bash
FROM    node:9-alpine
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
RUN     npm install
EXPOSE  80
CMD     ["node", "server.js"]
```

This configuration adds a Node.js application to the `node:9-alpine` image. Then configures the container to serve the application on port 80 via the *EXPOSE* instruction.

Now run the Azure CLI command, `az acr build`, to build the container image from the Dockerfile.

```azurecli
az acr build --registry <acrName> --image helloacrbuild:v1 .
```

You'll see the image being built and pushed to your Container Registry as you run the command.

## Verify the image

Run the following command to verify that the image has been created and stored in the registry.

```azurecli
az acr repository list --name <acrName> --output table
```

The output should look similar to the following:

```console
Result
-------------
helloacrbuild
```

The `helloacrbuild` image is now ready to be used.
