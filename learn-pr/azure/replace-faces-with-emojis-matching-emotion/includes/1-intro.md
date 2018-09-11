TheMojifier is a Slack _slash_ command which replaces peoples faces in images with emojis matching their emotion, like so:

![Example image](/media-drafts/example-mojify-image.png)

It's designed to work from Slack as a custom command, you can name the command how you want, for this document I've named it `mojify`.

To execute the commmand type `/mojify <image to mojify>`, like so:

![Example Image](/media-drafts/9.slack-type-mojify.png)

The mojifier then:

1.  Calculates the emotion of any people in the image.
2.  Matches emotions to emojis.
3.  Replaces the faces with emojis.
4.  Posts the image back to Twitter as a reply.

It’s written using TypeScript and several Azure technologies including [Azure Functions](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai) and [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

In this tutorial I’m going to explain how TheMojifier was made and show you how to create your own Slack command using Azure technologies.

> TODO, where will this be now?
> All the code for Mojifier is available on [GitHub](https://github.com/jawache/mojifier)

# Requirements

To build the mojifier, we need to use several Azure services.

## Azure Cognitive Services

Azure Cognitive Services are a set of high-level APIs you can use to add advanced AI functionality into your application quickly. If you can make an HTTP request, you can use Cognitive Services.

[More info](https://azure.microsoft.com/services/cognitive-services/?WT.mc_id=mojifier-sandbox-ashussai)

## Azure Functions

As powerful as Logic Apps are sometimes you need to write business logic using the full expressiveness of a programming language. Azure Functions is a technology that lets you host snippets of code that can respond to events or HTTP requests, Azure handles all of the scaling issues for you and you only pay for what you use.

[More info](https://azure.microsoft.com/services/functions/&WT.mc_id=mojifier-sandbox-ashussai)
