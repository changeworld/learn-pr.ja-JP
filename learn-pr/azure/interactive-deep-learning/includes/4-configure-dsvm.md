## Introduction to Jupyter for more interactive deep learning 

Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.

## Serving Jupyter Notebooks with Nvidia Docker on an Azure DSVM

### Step 1 Create a Linux DSVM

Use call code from the Azure CLI

```
code .
```

Fill in the following deployment schema and save it as parameter_file.json

``` 
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "YOURUSERNAME FOR NEW DSVM"},
     "adminPassword": { "value" : "PASSWORD FOR NEW DSVM"},
     "vmName": { "value" : "HOSTNAME OF DSVM"},
     "vmSize": { "value" : "VM SIZE For eg: Standard_DS2_v2"},
  }
}
```

A list of available vm sizes can be found here [Ubuntu DSVM ARM template](https://azure.microsoft.com/en-us/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).


### Create a resource group for your DSVM in a region of your choice:
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

A list of available regions can be found here [Azure Regions](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).

### Deploy your DSVM to your new resource group

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## Step 2 Open the Port 8888, 22 on the DSVM 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

Port 8888 is the default port for Jupyter Notebooks For detailed steps on opening a port [click here](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)
 
## Step 3 Connect to the DSVM with the Azure Shell 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## Step 4 Run Jupyter in Docker Container & link 8888 port to the VM Host 

Link port 8888 between the VM and the docker container, install jupyter and pull pytorch tutorials.  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## Step 5 Navigate to the Jupyter Notebook in the Browser 

Once the Jupyter notebook is running you will see a message as follows : 

> Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}

Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser:
- [[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}
