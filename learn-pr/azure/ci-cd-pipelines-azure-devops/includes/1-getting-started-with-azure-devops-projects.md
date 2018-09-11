Setting up a CI/CD pipeline has always been challenging to do quickly. Now, it is incredibly easy to go from nothing at all to a full end to end Azure DevOps project. And in the Azure DevOps project, you get the following:

1. Infrastructure provisioned for you in Azure.

2. A Team Project in a VSTS instance.

3. Source code for a sample app in the language that you picked in the repo of your VSTS instance.

4. A build and release pipeline that makes sense for the technologies picked.

And when its's done, the Azure DevOps project takes the sample app, builds and releases it through the pipelines into the infrastructure it provisions for you up in Azure. And you get all of this with just a couple of clicks.

## Create an Azure DevOps project

You create an Azure DevOps project from the Azure portal.

1. Head to the [Azure Portal](https://portal.azure.com) and click `Create a resource`  
![](/media-draft/1-azureportal.png)

2. Click `DevOps Project`  
![Pick DevOps Project](/media-draft/1-pickdevopsproject.png)

3. The next screen is where you get to pick what language you want to use. Notice how you can choose .NET (of course), java, node, php, python, ruby, and go. You can even bring your own code in from a git repo. For this unit, let’s go ahead and create a Node.js app. Click Node.js and click Next  
![Pick Node.js for language](/media-draft/1-picknodejsforlang.png)

4. Next it's going to ask you what node.js framework you want to use. For this unit, pick Simple Node.js app and click Next  
![Pick Simple Node](/media-draft/1-picksimplenode.png)

5. Next, it's going to ask you what infrastructure do you want to provision and run your app in? For this unit, let’s provision and deploy into a Kubernetes cluster using Azure Kubernetes Service. Select Kubernetes Service and click Next  
![Pick Kubernetes](/media-draft/1-pickkubernetes.png)

6. And now, you can either create a brand new instance of VSTS or choose an existing one. You also get to set up where and how you want your kubernetes cluster to run. For this unit, go ahead and create a new VSTS instance by choosing Select new and give your VSTS instance a unique name. Enter Learn for the Project name, pick your azure subscription, name the Cluster Name LearnCluster, select East US for the location, and click Done  
![Final Confirmation Screen](/media-draft/1-finalconfirmation.png)

And that is literally it! This takes a while so kick back, relax and just let Azure do its thing. Most of the time will be spent provisioning and configuring your Azure infrastructure. For this module, this will be your Azure Kubernetes Service and Azure Container Registry.

## A lap around the finished Azure DevOps Project

When Azure is done, you will be notified in your Alerts

1. Click on the alert and then Go to resource  
![Go To Resource From Alert](/media-draft/1-gotoresourcefromalert.png)

2. This takes you to a portal blade that displays everything provisioned. On the left-hand side is your CI/CD pipeline. You have your code repository, your build definition, and also your release definition. All the links are deep links that take you directly into the resource in VSTS. And on the right-hand side, you see all the infrastructure deployed for you in Azure. You have your Kubernetes cluster with your sample app already deployed and also application insight. Once again, all these links are deep links to the resources in Azure.  
![Portal DevOps Project](/media-draft/1-pickdevopsproject.png)

3. Click on the link for your source code  
![Link to Source](/media-draft/1-linktosource.png)

4. This takes you to the git repo in your VSTS project. Notice this is just a normal git repo holding the sample node.js app with helm charts.  
![VSTS Repo](/media-draft/1-vstsrepo.png)

5. Go into the builds  
![Link to Builds](/media-draft/1-navtobuild.png)

6. Edit the created build by clicking on the build and then click Edit  
![](/media-draft/1-editbuildlink.png)

7. You will see a build pipeline that makes sense for the technologies picked. Since we picked a node.js app into a Kubernetes cluster, we get a build pipeline that uses Docker tasks to build the Node.js app, create the image container image and then Helm tasks to create a Helm package.  
![Build Pipeline](/media-draft/1-buildpipeline.png)

8. Go to the Release pipeline by clicking `Releases`  
![Go to Releases](/media-draft/1-gotoreleases.png)

9. You'll see the release pipeline created. Edit it by clicking on the release and selecting `Edit pipeline`  
![Edit Release](/media-draft/1-editrelease.png)

10. Azure has created a release pipeline that makes sense for the technologies picked. A node app running in a container hosted in a Kubernetes cluster.
![Release Pipeline](/media-draft/1-releasepipeline.png)

11. Go back to the Azure portal and click on the external endpoint for the kubernetes service  
![](/media-draft/1-clickonendpoint.png)

12. You should see the sample app build and deployed into your AKS cluster  
![App Running](/media-draft/1-apprunning.png)

## Summary

In this unit, you created an Azure DevOps project that consists of:

1. Infrastructure for your app - Azure Kubernetes Service and Application Insight

2. A Team Project in a VSTS instance.

3. Source code for a Node.js sample app running in a container in the repo of your VSTS instance.

4. A build and release pipeline for a Node.js container app running in your Azure Kubernetes Service instance.

Next, learn how to replace the sample app with your real app.