# Anti-Garbage-Childrens-Game


+About Anti Garbage Childrens Game:
Small garbage on road also causes pollution,so to clean pollution while enjoying the process childern can use toy cars,drones,etc to collect garbage.First of all using Azure AI/ML the garbage is scanned through the cameras of toy cars and drones then the images will go in backed service to test folders for prediction later on children/user can collect the garbage by small robotic arms if available or manually and place the garbage in bin cans.This way children will get awareness about climate and may get fun.


+Discription:

This project is intended for the backend service to the Anti Garbage Childrens Game UI Applications of different platforms like for windows app,android app,web app.

There are many azure services used in this porject:
1)Azure Virtual Machine
Here i created a windows 10 pro virtual machine to deploy my code and run it continuously on cloud with interruptions and i have accessed the vm by rdp file.
2)In Azure AI/ML :-Azure Custom Vision
By using this service i trained machine learning models
here i used both Training and Prediction service
3)Azure storage
The large storage is needed for the storage of various garbage images.

+Deployment
1)The project is created in c# lanaguage and .net framework

2)The project contains directories namely C-Sharp and training-images
a)C-Sharp:This directory has two sub-directories namely train-classifier and test-classifier where in train-classifier there is appsettings.json file which is used to store the ProjectID,TrainingKey and TrainingEndpoint from the custom vision service of model and also the program code and training images according to categories.Later in test-classifier there is appsettings.json file which is used to store the ProjectID,PredictionKey,PredictionEndpoint and ModelName and also the program code and the test-images folder where the images from Iot enabled toys will be stored.
b)training-images:It is a directory which hs large amounts of garbage images according to categories which can be added to train images folder for more learning of machine.

3)After deploying the code in proper location i have installed 2 packages for custom vision service linking to local deployed code into going respective folders and running on terminal of vscode.
a)Package for train-classifier:"dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training --version 2.0.0"
b)Package for test-classifier:"dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction --version 2.0.0"

4)we can run the code by "dotnet run" command which will upload and train the model with new iteration in azure custom vision service and during prediction it will use the model and give result by same command.

+Implementation:

Classify Images with Custom Vision
In this demo, i will use the Custom Vision service to train an image classification model that can identify six classes of garbage (cardborard,glass,metal,paper,plastic,trash).


1)Clone the repository

1.	Start Visual Studio Code.
2.	Open the palette and run a Git: Clone command to clone the https://github.com/riggod/Anti-Garbage-Childrens-Game repository to a local folder.
3.	When the repository has been cloned, open the folder in Visual Studio Code.
4.	Wait while additional files are installed to support the C# code projects in the repo.


2)Create Custom Vision resources
Before we can train a model, we will need Azure resources for training and prediction. we can create Custom Vision resources for each of these tasks, or we can create a single Cognitive Services resource and use it for either (or both).
In this demo, we will create Custom Vision resources for training and prediction so that we can manage access and costs for these workloads separately.
1.	In a new browser tab, open the Azure portal at https://portal.azure.com, and sign in using the Microsoft account associated with Azure subscription.
2.	Select the ＋ Create a resource button, search for custom vision, and create a Custom Vision resource with the following settings:
o	Create options: Both
o	Subscription: Azure subscription
o	Resource group: Choose or create a resource group
o	Region: UK South
o	Name: MSProject
o	Training pricing tier: F0
o	Prediction pricing tier: F0
3.	Wait for the resources to be created, and then view the deployment details and note that two Custom Vision resources are provisioned; one for training, and another for prediction. we can view these by navigating to the resource group where we created them.


3)Create a Custom Vision project
To train an image classification model, we need to create a Custom Vision project based on training resource. To do this, we will use the Custom Vision portal.
1.	In Visual Studio Code, view the training images in the Anti-Garbage-Childrens-Game\training-images folder where we cloned the repository. This folder contains subfolders of cardborard,glass,metal,paper,plastic and trash images.
2.	In a new browser tab, open the Custom Vision portal at https://customvision.ai. If prompted, sign in using the Microsoft account associated with Azure subscription and agree to the terms of service.
3.	In the Custom Vision portal, create a new project with the following settings:
o	Name: Classify Garbage
o	Description: Image classification for Garbage
o	Resource: The Custom Vision resource we created previously
o	Project Types: Classification
o	Classification Types: Multiclass (single tag per image)
o	Domains: General
4.	In the new project, click [+] Add images, and select all of the files in the training-images/cardboard folder we viewed previously. Then upload the image files, specifying the tag cardborad, like this:

![customvisioncard](https://user-images.githubusercontent.com/91531884/175301822-5feaf628-d906-469a-b6c2-8e0597b26c32.jpg)


 
5.	Repeat the previous step to upload the images in the glass folder with the tag glass, and same with other folders too.
6.	Explore the images we have uploaded in the Custom Vision project - there should be 15 images of each class, like this:
 
7.	In the Custom Vision project, above the images, click Train to train a classification model using the tagged images. Select the Quick Training option, and then wait for the training iteration to complete.
8.	When the model iteration has been trained, review the Precision, Recall, and AP performance metrics - these measure the prediction accuracy of the classification model, and should all be high.The performance metrics are based on a probability threshold of 50% for each prediction (in other words, if the model calculates a 50% or higher probability that an image is of a particular class, then that class is predicted). We can adjust this at the top-left of the page.


4)Test the model
Now that we've trained the model, we can test it.
1.	Above the performance metrics, click Quick Test.
2.	Select browse lov=cal files or insert image url.
3.	View the predictions returned by model - the probability score for cardboard should be the highest, like this:
 
4.	Close the Quick Test window.


5)View the project settings
The project we have created has been assigned a unique identifier, which we will need to specify in any code that interacts with it.
1.	Click the settings (⚙) icon at the top right of the Performance page to view the project settings.
2.	Under General (on the left), note the Project Id that uniquely identifies this project.
3.	On the right, under Resources note that the key and endpoint are shown. These are the details for the training resource.


6)Use the training API

1.	In Visual Studio Code, in the Explorer pane, browse to the Anti-Garbage-Childrens-Game folder and expand the C-Sharp.
2.	Right-click the train-classifier folder and open an integrated terminal. Then install the Custom Vision Training package by running command "dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training --version 2.0.0"

3.	View the contents of the train-classifier folder, and note that it contains a file for configuration settings: "appsettings.json"

Open the configuration file and update the configuration values it contains to reflect the endpoint and key for Custom Vision training resource, and the project ID for the classification project we created previously. Save changes.
4.	Note that the train-classifier folder contains a code file for the client application: "Program.cs"

Open the code file and review the code it contains, noting the following details:
o	Namespaces from the package we installed are imported
o	The Main function retrieves the configuration settings, and uses the key and endpoint to create an authenticated CustomVisionTrainingClient, which is then used with the project ID to create a Project reference to project.
o	The Upload_Images function retrieves the tags that are defined in the Custom Vision project and then uploads image files from correspondingly named folders to the project, assigning the appropriate tag ID.
o	The Train_Model function creates a new training iteration for the project and waits for training to complete.
5.	Return the integrated terminal for the train-classifier folder, and enter the following command to run the program: "dotnet run"
6.	Wait for the program to end. Then return to browser and view the Training Images page for project in the Custom Vision portal.
7.	Verify that some new tagged images have been added to the project. Then view the Performance page and verify that a new iteration has been created.


7)Publish the image classification model

Now we're ready to publish trained model so that it can be used from a client application.
1.	In the Custom Vision portal, on the Performance page, click 🗸 Publish to publish the trained model with the following settings:
o	Model name: Iteration4
o	Prediction Resource: The prediction resource we created previously.
2.	At the top left of the Project Settings page, click the Projects Gallery (👁) icon to return to the Custom Vision portal home page, where project is now listed.
3.	On the Custom Vision portal home page, at the top right, click the settings (⚙) icon to view the settings for Custom Vision service. Then, under Resources, find prediction resource to determine its Key and Endpoint values.


8)Use the image classifier from a client application
Now that we've published the image classification model, we can use it from a client application.
1.	In Visual Studio Code, in the Anti-Garbage-Childrens-Game, in the subfolder C-Sharp right-click the test-classifier folder and open an integrated terminal. Then enter the SDK-specific command to install the Custom Vision Prediction package: "dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction --version 2.0.0"
2.	Expand the test-classifier folder to view the files it contains, which are used to implement a test client application for image classification model.
3.	Open the configuration file for client application (appsettings.json) and update the configuration values it contains to reflect the endpoint and key for Custom Vision prediction resource, the project ID for the classification project, and the name of published model. Save changes.
4.	Open the code file for client application (Program.cs for C#) and review the code it contains, noting the following details:
o	Namespaces from the package we installed are imported
o	The Main function retrieves the configuration settings, and uses the key and endpoint to create an authenticated CustomVisionPredictionClient.
o	The prediction client object is used to predict a class for each image in the test-images folder, specifying the project ID and model name for each request. Each prediction includes a probability for each possible class, and only predicted tags with a probability greater than 50% are displayed.
5.	Return the integrated terminal for the test-classifier folder, and enter the SDK-specific command to run the program: "dotnet run"
6.	View the label (tag) and probability scores for each prediction. We can view the images in the test-images folder to verify that the model has classified them correctly.

+Thank you for your precious time for reading this readme 


