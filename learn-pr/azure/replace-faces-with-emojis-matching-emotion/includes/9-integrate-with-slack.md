We have created all the required Azure functions and we can run them locally using the `func host start` command.

In this lecture we will connect our Azure Functions together with slack and create a slack _slash_ command which will call our Azure Functions and display the resulting mojified in the slack window.

## Using ngrok to develop with slash commands

Our Azure functions are currently only running locally, however slack runs in the cloud, on the internet.

When we setup a slack _slash_ command in the next section it's going to ask for a public URL which it will call whenever someone executes a slash command.

We want to give them our `RespondToSlashCommand` URL, but that only exists on our local computer.

How can you give services in the cloud access to resources on your local computer for development?

A great solution to this conundrum is `ngrok` this is a tool whick creates secure tunnels to your local machine from the internet.

1. Visit https://ngrok.com/download
2. Follow the instructions to download and install the `ngrok` package to your local machine.
3. Run `ngrok http 7071` in a terminal.
   ![ngrok](/media-drafts/9.ngrok.png)
4. This will print out a public URL ending in `ngrok.io` e.g. `http://bbade778.ngrok.io`, make a note of this you will need it in the next section.

> NOTE: This `http://*.ngrock.io` URL you can use just as you used `http://localhost:7071`, try it out with an image like so `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`

## Create a slack app

A slash command exists as part of a slack app, a slack _bot_.

Visit [Create Slack App](https://api.slack.com/apps/new)

Choose an `App Name` and associate it with the `Workspace` you created at the start of this tutorial and then press `Create App`, like so:

![Create Slack App](/media-drafts/9.create-slack-app.png)

Once that's created we then need to go into our slack app and create a slash command.

## Create a slash command

Click on the `Slash Commands` menu item

![Slash Commands](/media-drafts/9.slash-commands.png)

Click `Create New Command`

- Enter whatever name you want for your slash command in the `command` field.
- Enter the ngrok public url into the `Request URL` field

  > Remember to append `/api/RespondToSlashCommand` to the URL

- Add a short description and usage hint.
- Press `Save`

![Slash Commands](/media-drafts/9.create-slash-command.png)

Once saved you should see a screen like so:

![Slash Commands Success](/media-drafts/9.create-slash-commands-success.png)

## Install slack app to workspace

Before we can use our slack app in our workspace we need to install it.

1. Select the `Basic Information` menu item

2. Expand out the `Install your app to your workspace` option and press the `Install app to workspace` button.

   ![Install App To Workspace](/media-drafts/9.install-app-to-workspace.png)

3. It may ask you to authenticate your application like so:

   ![Authenticate App To Workspace](/media-drafts/9.authenticate-slack-app.png)

## Try it out

Now that your slash command has been created and connected to our locally running instance of Azure Functions via ngrok link, let's test it.

1. Go to the slack workspace you associated with your slack app.
2. Go to any chat window and type `/mojify` (or whatever name you gave the app)
3. If everything has worked correctly you should see `mojify` as an option

   ![Check Mojify](/media-drafts/9.slack-check-mojify.png)

4. Now just type `/mojify` and add an image url then press enter, like so:

   ![Type Mojify](/media-drafts/9.slack-type-mojify.png)

<!-- A few things should happen at this point, if you look at the vs code console you should see a log line for a request to http://localhost:7071/api/RespondToSlackCommand

This is the Azure Function reponding to the initial slash command from Slack, we return the image we want them to display.

You should soon see a second log line for http://localhost:7071/api/MojifyImage, this is Slack requesting the image to display.

In the slack window you should see the mojified image, like so:

> TODO IMAGE

> NOTE
> If something went wrong one of the advantages of Azure Functions is that it's running locally, in debug mode. Simply drop a break point in the relevant function and debug your way through. -->
