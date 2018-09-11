Later in this module, you will create a Node.js app that uses the model to identify the artist of paintings presented to it. But you don't have to write an app to test the model; you can do your testing in the portal, and you can further refine the model using the images that you test with. In this unit, you will test the model's ability to identify the artist of a painting using test images provided for you.

1. Click **Quick Test** at the top of the page.

    ![Testing the model](../media/4-portal-click-quick-test.png)

1. Click **Browse local files**, and then browse to the "Quick Tests" folder in the module resources. Select **PicassoTest_01.jpg**, and click **Open**.

    ![Selecting a Picasso test image](../media/4-portal-select-test-01.png)

1. Examine the results of the test in the "Quick Test" dialog. What is the probability that the painting is a Picasso? What is the probability that it is a Rembrandt or Pollock?

1. Close the "Quick Test" dialog. Then click **Predictions** at the top of the page.

    ![Viewing the tests that have been performed](../media/4-portal-select-predictions.png)

1. Click the test image that you uploaded to show a detail of it. Then tag the image as a "Picasso" by selecting **Picasso** from the drop-down list and clicking **Save and close**.

    > By tagging test images this way, you can refine the model without uploading additional training images.

    ![Tagging the test image](../media/4-tag-test-image.png)

1. Perform another quick test using the file named **FlowersTest.jpg** in the "Quick Test" folder. Confirm that this image is assigned a low probability of being a Picasso, a Rembrandt, or a Pollock.

The model is trained and ready to go and appears to be adept at identifying paintings by certain artists. Now let's go a step further and incorporate the model's intelligence into an app.