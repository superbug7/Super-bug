# Super-bug
AI based bug management system 


SUPERBUG: AI BUG MANAGEMENT SYSTEM by
Gaurav Shad & Rohit Taneja
July,2017

CONTENTS
I. FILES ATTACHED
II. HOW TO SETUP AND TEST
III. ALEXA INTEGRATION

I. FILES ATTACHED

superbug-classifier.ipynb ( source code in jupyter notebook format for modularity )
model.h5/json ( trained model with LSTM : 75% accuracy for 4 classes)
Superbug_project.pdf ( Project description, HLD, Software design and sources used for this project)
Docker image with full environment available here: https://hub.docker.com/r/superbug/bug_classifier/ . Files are located in $~/superbug inside container
Alexa integration source code ( alexa-skills-EHR.zip ) is inside docker container

II. How to Setup and test

For loading the environment to train the network or do inference, we are providing the docker container with the environment setup. Below are instructions for setting in up:
a. Pull the docker image from https://hub.docker.com/r/superbug/bug_classifier/  ( tag: version 4 )
b. Install nvidia-docker from https://github.com/NVIDIA/nvidia-docker
c. Get inside the pulled container using this command:
i. sudo nvidia-docker run -ti -p 8888:8888 bug_classifier bash
ii. Inside the container, run $source active dl_env to activate python environment
iii. jupyter notebook --ip 0.0.0.0 --no-browser --allow-root ( this will give you the URL to open in browsere on Host )
iv. Open the browser in host and copy the URL to see the notebook
v. You can navigate to superbug_classifier.ipynb notebook to look in the source code

We are also packaging the trained model files ( model.h5 ) and source code ( superbug_classifier.ipynb ) which can be opened in jupyter notebook

We also have inference engine in super-bug.com/demo where there are some example bug description files that you can paste in Bug window and try to see if classifies it right or not.

III. ALEXA INTEGRATION

To run this skill, you need to do two things. The first is to deploy the example code in lambda, and the second is to configure the Alexa skill to use Lambda. AWS Lambda Setup 1. Go to the AWS Console and click on the Lambda link. Note: ensure you are in us-east or you wont be able to use Alexa with Lambda. 2. Click on the Create a Lambda Function or Get Started Now button. 3. Skip the blueprint 4. Name the Lambda Function "Superbug". 5. Select the runtime as Java 8 6. Go to the the /alexa-skills-EHR/ directory containing pom.xml, and run 'mvn assembly:assembly -DdescriptorId=jar-with-dependencies package'. This will generate a zip file named "alexa-skills-kit-samples-1.0-jar-with-dependencies.jar" in the target directory. 7. Select Code entry type as "Upload a .ZIP file" and then upload the "alexa-skills-kit-samples-1.0-jar-with-dependencies.jar" file from the build directory to Lambda 8. Set the Handler as EHRSystem.EHRsystemSpeechletRequestStreamHandler (this refers to the Lambda RequestStreamHandler file in the zip). 9. Create a basic execution role and click create. 10. Leave the Advanced settings as the defaults. 11. Click "Next" and review the settings then click "Create Function" 12. Click the "Event Sources" tab and select "Add event source" 13. Set the Event Source type as Alexa Skills kit and Enable it now. Click Submit. 14. Copy the ARN from the top right to be used later in the Alexa Skill Setup. Alexa Skill Setup 1. Go to the Alexa Console and click Add a New Skill. 2. Set "Superbug" as the skill name and "Superbug" as the invocation name, this is what is used to activate the skill. For example you would say: "Alexa, start superbug”. 3. Select the Lambda ARN for the skill Endpoint and paste the ARN copied from above. Click Next.
Superbug – AI bug management system July 2017
9

Copy the Intent Schema from the included IntentSchema.json. 5. Copy the Sample Utterances from the included SampleUtterances.txt. Click Next. 6. Go back to the skill Information tab and copy the appId. Paste the appId into the EHRSystemSpeechletRequestStreamHandler.java file for the variable supportedApplicationIds, then update the lambda source zip file with this change and upload to lambda again, this step makes sure the lambda function only serves request from authorized source. 7. You are now able to start using the skill! You should be able to go to the Echo webpage and see the skill enabled. 8. In order to test it, try to say some of the Sample Utterances from the Examples section below. 9. The skill is now saved and once you are finished testing you can continue to use it. Examples One-shot model: User: "Alexa, start superbug." Alexa: "Welcome to superbug!"
