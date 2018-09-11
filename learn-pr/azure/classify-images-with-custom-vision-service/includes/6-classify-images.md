In this unit, you will use the Artworks app to submit images to the Custom Vision Service for classification. The app uses the JSON information returned from calls to the Custom Vision Prediction API's [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) method to tell you whether an image represents a painting by Picasso, Rembrandt, Pollock, or none of the above. It also shows the probability that the classification assigned to the image is correct.

1. Click the **Browse (...)** button in the Artworks app.

    ![Browsing for local images in the Artworks app](../media/6-app-click-browse.png)

1. Browse to the "Quick Tests" folder in the module resources. Select the file named **PicassoTest_02.jpg**, and then click **Open**.

1. Click the **Predict** button to submit the image to the Custom Vision Service.

    ![Submitting the image to the Custom Vision Service](../media/6-app-click-predict.png)

1. Confirm that the app identifies the painting as a Picasso.

    ![Classifying an image as a Picasso](../media/6-app-prediction-01.png)

1. Repeat steps 1 through 3 for **RembrandtTest_01.jpg** and **PollockTest_01.jpg**, and confirm that the app can identify paintings by Rembrandt and Pollock.

    ![Classifying an image as a Rembrandt](../media/6-app-prediction-02.png)

1. Repeat steps 1 through 3 for **VanGoghTest_01.png** and **VanGoghTest_02.png**, and confirm that the app does not identify these Van Gogh masterworks as paintings by Picasso, Rembrandt, or Pollock.

    ![Not a Picasso, Rembrandt, or Pollock](../media/6-app-prediction-03.png)

1. As you can see, using the Prediction API from an app is as reliable as it is through the Custom Vision Service portal — and way more fun! What's more, if you go to the Predictions page in the portal, you'll find each of the images uploaded via the app shown there as well.

    ![Image submitted to the Custom Vision Service](../media/6-portal-all-predictions.png)

Feel free to test with images of your own and gauge the model's adeptness at identifying artists or determining that an image is not a Picasso, Rembrandt, or Pollock. If you'd like to train it to recognize Van Goghs too, upload some Van Gogh paintings, tag them with "Van Gogh," and retrain the model. There is no limit to the intelligence you can add if you're willing to do the training. And remember that in general, the more images you train with, the smarter the model will be.