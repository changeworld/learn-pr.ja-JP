After creating an Azure DevOps project, the first thing everyone ask is how do you replace the sample app with your own app? It's quite simple and in this unit, you will learn two ways to do it.

1. Replacing the code in the VSTS git repository with your real code

2. Pointing your build pipeline to an external git repo holding your real code

## Replacing code in VSTS git repository

One simple way is by cloning the git repo in VSTS onto your hard drive, replacing everything with your own code, uploading back to VSTS and voila. Your code will now be built and deployed through the CI/CD pipeline.

For this unit, you will start by downloading and storing the source code to the node.js app onto your hard drive.

1. Download the source code from <https://abelsharedblob.blob.core.windows.net/microsoftlearn/MicrosoftLearnDevOps.zip>

2. Extract the contents of MicrosoftLearnDevOps.zip somewhere on your hard drive. For this example `C:\users\abel\Downloads\MicrosoftLearnDevOps` was used  
![Unzipped Directory](/media-draft/2-unzippedfolder.png)

Next, you need to clone the repo onto your hard drive and replace the sample app with the real node.js app. This unit assumes you already have git installed on your computer.

1. From Azure portal browse to your Azure DevOps Project and click on the code repository link.  
![](/media-draft/2-browsetorepolink.png)

2. Click on Clone and copy the url for the git repo in the upper right-hand side.  
![Copy Clone Url](/media-draft/2-copycloneurl.png)  
This will copy the repo url to your clipboard

3. Clone the repo to your hard drive  
![Git Clone](/media-draft/2-gitclone.png)  
In this example the repo was cloned to C:\Users\abel\Source\TripleCrown\DevOps

4. Delete everything from your local repo except for the `.git` directory  
![Delete Repo of Everything](/media-draft/2-deleterepoofeverything.png)

5. Copy the source code for the downloaded node.js app into the repo folder  
![Replaced Code](/media-draft/2-replacedeverything.png)

6. Add all the changes to your local repo by typing `git add *` from the command line  
![Git Add All](/media-draft/2-gitaddall.png)

7. Commit changes to your local repo by typing `git commit -m "replace sample app with real code"`  
![Git Commit](/media-draft/2-gitcommit.png)

8. Push changes back to the git repo in VSTS with `git push`  
![Git Push](/media-draft/2-gitpush.png)

9. After pushing changes back to VSTS, this should send the real app code through the build  
![Build Kicked Off](/media-draft/2-buildkickedoff.png)  
![Build in Action](/media-draft/2-buildinaction.png)
 and release pipeline all the way to azure  
 ![Release Running](/media-draft/2-releaserunning.png)

 After the deployment finishes, you can verify the real app was deployed by going back to the Azure portal

 1. Go to the Azure portal, browse to your Azure DevOps project, and click on your deployed app on the right-hand side  
 ![Launch Sample App Link](/media-draft/2-launchapp.png)

 2. This launches the running app in your browser  
 ![App Running](/media-draft/2-apprunning.png)

## Using external git repo

Another way to swap out the sample app with your real app code is by pointing the build pipeline to an external git repository that holds your app code. For this example, upload the real app code to a github repository.

After uploading the real code to github, do the following to point the build pipeline to this github repository

1. From the Azure portal, browse to your Azure DevOps project and click on the build link  
![Build Link](/media-draft/2-buildlink.png)

2. This takes you to the build pipelines, click your build pipeline and then `Edit`  
![Click Edit Build](/media-draft/2-clickeditbuildlink.png)

3. This takes you to the build editor, click on `Get sources`  
![Click Get Source](/media-draft/2-clickgetsource.png)

4. This takes you to the Select a source page. Notice how you can not only use VSTS Git, but also GitHub, GitHub Enterprise, Subversion, Bitbucket Cloud, and any external Git based repo for the build pipeline. For this exercise, select GitHub  
![Select GitHub](/media-draft/2-selectgithub.png)

5. This takes you to the GitHub connection page. Either use OAuth or a Personal Access Token to connect with your GitHub account

6. Select the github repo holding the real app code and click `Save & queue`  
![Save and Queue](/media-draft/2-saveandqueue.png)

7. Click the `Save & queue` button  
![](/media-draft/2-saveandqueuedialog.png)

8. This action saves and kicks off the build, sending the real app code hosted in GitHub through our build and release pipelines all the way to Azure  
![Build Running](/media-draft/2-buildrunning.png)

## Summary

In this unit, you learned two different ways to replace the sample code in the DevOps project with real app code. This can be done either by replacing the code in the VSTS git repo, or by linking the build pipeline with another external repo which holds your app code.

Next learn how to customize the build and release pipelines.