You've successfully created a CI/CD pipeline for your custom application using Azure DevOps projects. 

## Clean up resources

When you're done working with this CI/CD pipeline, you can delete all resources created during the tutorial.

1. From the Azure portal, browse to `Resource Groups` and click on `VstsResourceGroup-LearnCluster-xxxx` where xxx is random groups of letters and numbers  
![LearnClusterRG](/media-draft/4-learnclusterrg.png)

2. Click on the DevOps project named `Learn`  
![Learn Link](/media-draft/4-learnlink.png)

3. Click on `Delete`  
![Delete](/media-draft/4-deleteproj.png)

4. Click on `Yes`  
![Yes](/media-draft/4-yes.png)

This will delete all resource created in Azure.

## Summary

In this tutorial, you learned how to:
> [!div class="checklist"]
> * Create an Azure DevOps project
> * Replace the sample app in the Azure DevOps project with your own
> * Customize the Buld and Release pipelines
