The out of the box experience with an Azure DevOps Project creates build and release pipelines that make sense for the technologies picked. For this module, you created a build and release pipeline that makes sense for a node.js app running in a container hosted in a Kubernetes cluster. 

Often times we need to customize the build and release pipelines to do specific things for our project. The build and release pipelines in VSTS are 100% customizable. You can make the pipelines do whatever you need it to do.

In this unit, learn how to customize your build and release pipelines.

## Customize the build pipeline

The build engine in VSTS is just a task runner, doing one task, after another. To customize the build, you just add or remove tasks and fill out the correct parameters for the task.

Out of the box, VSTS comes with about 100 tasks that you can use. If you need to do something that doesn't exist out of the box, check the marketplace where there are over 700 build and release tasks ready to be downloaded and used. You also have the ability to write your own custom tasks.

For this unit, you will customize the build pipeline by installing the marketplace tasks WhiteSource Bolt to do security scanning of our code.

1. Browse to the Azure DevOps Project in the Azure portal and click on the build definition link  
![Build Link](/media-draft/3-buildlink.png)

2. This takes you to the build pipelines page. Click on the build and select `Edit`  
![Edit Build](/media-draft/3-editbuild.png)

3. This takes you to the build definition. Click on the `+` to add a task to Agent Job 1  
![Add Task](/media-draft/3-addtask.png)

4. In the text field, type `bolt` and click `Get it free`  
![Get it free](/media-draft/3-getitfree.png)

5. This takes you to the WhiteSource Bolt Marketplace page. Click `Get it free`  
![Get White Source Bolt Free](/media-draft/3-getwhitesourceboltfree.png)

6. Choose your VSTS organization and then click `Install`  
![Install](/media-draft/3-install.png)

7. Activate WhiteSource Bolt by following the directions here <https://www.whitesourcesoftware.com/whitesource_bolt_visualstudio_2017/#activate>

8. Go back to your Azure portal with the DevOps project loaded and click the build pipeline link  
![Build Pipeline Link](/media-draft/3-buildpipelinelink.png)

9. Select your build and click `Edit`  
![Edit Build](/media-draft/3-editbuild.png)

10. Click the `+` Add a task to Agent job 1, type in `bolt` in the search field and click `Add`  
![Add Bolt](/media-draft/3-addbolt.png)

11. This adds the WhiteSource Bolt task to the bottom of the task list, drag it to the top  
![Bolt at Top](/media-draft/3-boltattop.png)

12. Click `Save & queue` and select `Save & queue`  
![Save and Queue](/media-draft/3-saveandqueue.png)

13. Click `Save & queue`  
![Save and Queue Dialog](/media-draft/3-saveandqueuedialog.png)

This saves the modified build pipeline and queues the build. After the build finishes, looking at the build `WhiteSource Bolt Build Report`, you can see the source code was scanned by WhiteSource Bolt looking for security vulnerabilities.

![Build report](/media-draft/3-buildreport.png)

## Customize the release pipeline

Like the build, the release pipeline is task runner and can be customized the same way. For this unit, you will add a web performance test at the end of the release. This will verifying that your app is deployed and running successfully in the Kubernetes cluster.

1. Browse to the DevOps project in the Azure Portal and click on the link for the release pipeline  
![Release Link](/media-draft/3-releaselink.png)

2. This takes you to the release pipeline page. Click your release pipeline and click on `Edit`  
![Edit Release Pipeline](/media-draft/3-editreleasepipeline.png)

3. Click on the tasks in your release `Dev` stage  
![Release Stage](/media-draft/3-releasestage.png)

4. Click on the `+` Add a task to Phase1, type `web test` in the search field and click `Add` for the Cloud-based Web Performance Test task  
![Add Web Test](/media-draft/3-addwebtest.png)

5. Edit the Quick Web Performance Test task by clicking on it and adding the url to your app in the Website URL (to find the url, go to the Azure portal DevOps project page and on the right-hand side, right-click your sample app external endpoint and copy link) and then for TestName, enter in `Ping Test`  
![Copy URL](/media-draft/3-copyurl.png)  
![Edit Web Test Task](/media-draft/3-editwebtesttask.png)

6. Click `Save`  
![Save Release](/media-draft/3-saverelease.png)

Now, when you run a release, after deploying the new helm package, a web test is run hitting the app url successfully.

![Web Test](/media-draft/3-webtest.png)


## Summary

In this unit, you learned how to customize your build and release pipelines. You also learned how to install and use tasks from the Marketplace.