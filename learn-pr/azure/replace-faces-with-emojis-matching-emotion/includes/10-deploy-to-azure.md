So far in this module we've managed to get a slack _slash_ command working but only whilst running on localhost and by tunneling onto our computer using ngrok. This is fine for development but to release we want our code to be running in the cloud.

In this lecture we are going to deploy our Azure Function to the cloud and update the slack configuration to use the new production version of our code.

## Create Azure Function

We are going to be creating the azure function using VS Code and the Azure Function Extension.

1. Open the command palette and select "Azure Function: Deploy Function App".

> TODO: Image

2. It will ask you to confirm you want to upload the current project, just go ahead and select that.

> TODO: Image

3. Choose create new function app

> TODO: Image

4. Choose create new resource group.

> TODO: Image

5. Choose create new storage account.

> TODO: Image

6. Choose name.

> TODO: Image

7. Wait for the upload to complete, once complete the output window will show a URL like so:

> TODO: Image

## Try it out

At this point we should at least expect the `RespondToSlackCommand` function to be working since it doesn't rely on any other dependancies.

Visit `[deployed-app-name].azurefunctions.com/api/RespondToSlashCommand?text=[image-url]`

If all is working well then you should some json returned in the browser window like so:

> TODO: Image

If it's not working then try troubleshooting the issues here.

## Upload Settings

Remember we have some settings we put in `localsettings.json` these were the keys and URL for cognitive services.

`localsettings.json` is exactly that, local, it's never copied over to production when you deploy.

Normally you would need to head over to the portal and perhaps manualy add in the settings via the UI or use the `func` CLI to do the same.

However since we are using vs code we can use the Azure Func Extension and the command palete to upload the local settings for us.

1. Open the command palette with Ctrl+P
2. Select Azure: Upload Local Settings.
3. When the command completes you should see the below output in the VS Code output window.

## Try it out

Now if everything is working as expected then now the MojifyImage endpoint should be working.

Visit `[deployed-app-name].azurefunctions.com/api/MojifyImage?imageUrl=[image-url]`

If all is working well then you should see the mojified image in the window.

If it's not working then try troubleshooting the issues here.

## Update Slack

1. Paste this URL into the Slash Commands window

Remember you need to append the `/api/RespondToSlashCommand` to the end for it to work

> TODO: Image

## Try it out

To check that everything is configured correctly and that Slach will be requesting data from the deployed app rather than the local app, let's just swtich off ngrok for now.

Then head to slack window and type your favourite slack slash command.

`/mojify <image>`

If it's all working as expected then we should see an image appear in the slack window
