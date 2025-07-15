## Basic setup

First, let's set up the environment by executing the following code:

```cmd
python3.11 -m venv my_env
source my_env/bin/activate # activate my_env
```

Run the following commands in the terminal to receive the outline of the project, rename it with another name and finally move into that directory:

```cmd
git clone https://github.com/ibm-developer-skills-network/translator-with-voice-and-watsonx
cd translator-with-voice-and-watsonx
```

## HTML, CSS, and JavaScript
The ```index.html``` file is responsible for the layout and structure of the web interface. This file contains the code for incorporating external libraries such as JQuery, Bootstrap, and FontAwesome Icons, as well as the CSS (`style.css`) and JavaScript code (```script.js```) that control the styling and interactivity of the interface.

The `style.css` file is responsible for customizing the visual appearance of the page's components. It also handles the loading animation using CSS keyframes. Keyframes are a way of defining the values of an animation at various points in time, allowing for a smooth transition between different styles and creating dynamic animations.

The `script.js` file is responsible for the page's interactivity and functionality. It contains the majority of the code and handles all the necessary functions such as switching between light and dark mode, sending messages, and displaying new messages on the screen. It even enables the users to record audio.

## Authenticating for programmatic access
In this project, you do not need to specify your own `Watsonx_API` and `Project_id` to the below `worker.py` code. You can just specify `project_id="skills-network"` and leave `Watsonx_API` blank, as in this CloudIDE environment, we have already granted you access to API without your own `Watsonx_API` and `Project_id`.

But it's important to note that this access method is exclusive to this Cloud IDE environment. If you are interested in using the model/API outside this environment (for example, in a local environment), detailed instructions and further information are available in this [tutorial](https://medium.com/the-power-of-ai/ibm-watsonx-ai-the-interface-and-api-e8e1c7227358).

## Deploy to Code Engine (OPTIONAL)
##### IMPORTANT NOTE: WHEN DEPLOYING YOU MUST USE YOUR OWN API KEY AND WATSONX PROJECT ID
If you would like to host your application and have it available for anyone to use, you can follow these steps to deploy it. The deployment will be to IBM Cloud's Code Engine. IBM Cloud Code Engine is a fully managed, cloud-native service for running containerized workloads on IBM Cloud. It allows developers to deploy and run code in a secure, scalable, and serverless environment, without having to worry about the underlying infrastructure.

The following steps in `Part 1` allow you to deploy to an IBM Skills Network Code Engine environment to test if everything is working just fine, which is deleted after a few days. `Part 2` shows the steps to deploy for real to your own account.

##### Part 1: Deploying to Skills Network Code Engine
###### Step 1. Create Code Engine project
In the left-hand navigation panel, there is an option for the Skills Network Toolbox. Simply open that and expand the CLOUD section and then click on Code Engine. Finally, click on Create Project.

###### Step 2. Click on Code Engine CLI button
From the same page simply click on Code Engine CLI button. This will open a new terminal and will login to a code engine project with everything already set up for you.

###### Step 3. Deploy Speech-to-Text service
From the same terminal that opened in the last step, run the following command to deploy Watson Speech-to-Text Service:

```cmd
ibmcloud ce application create --name speech-to-text \
  --env ACCEPT_LICENSE=true \
  --image us.icr.io/sn-labsassets/speech-standalone:latest \
  --port 1080 \
  --registry-secret icr-secret \
  --min-scale 1 \
  --visibility project
```

Parameter explanation:

- `--env` This argument allows you to pass in environment variables to your image. For the TTS and STT service we simply need to set ACCEPT_LICENSE to true.
- `--image` This specifies a pre-built image to run. Here you will use the skills network provided images.
- `--port` This lets Code Engine know what port the application runs on (it uses this for a health check). The TTS and STT service run on port 1080.
- `--registry-secret` This is allowing you to pass in an Image Registry secret (for access to private container images). Here you will use icr-secret which has been pre-created for you by Skills Network (and has access to all the images).
- `--min-scale` Code Engine can automatically scale the servers that run your application (More demand = More servers). This value default to 0 which means that when the application is not in use it will shut down all the servers (and won't incur charges anymore). Here you'll set it to 1 to ensure we always have a service available.
- `--visibility` This allows you to set who is able to access your project. Here we set project as this service will not be public and only used by the application you coded privately.

Once the service is ready, it will output a URL in the terminal. Go to your speech_to_text function defined in the `worker.py` file, and then replace the base_url with that.

###### Step 4. Deploy Text-to-Speech service
From the same terminal window, run the following command to deploy Watson Text-to-Speech Service:

Similarly, once the service is ready, it will output a URL in the terminal. Simply go to your `text_to_speech` function defined in the `worker.py` file, and then replace the `base_url` with that.

```cmd
ibmcloud ce application create --name text-to-speech \
  --env ACCEPT_LICENSE=true \
  --image us.icr.io/sn-labsassets/tts-standalone:latest \
  --port 1080 \
  --registry-secret icr-secret \
  --min-scale 1 \
  --visibility project
```

###### Step 5. Deploy your app
Finally, from the same terminal window (make sure you are in the `translator-with-voice-and-watsonx` directory), run the following command to deploy your app to Code Engine.

### IMPORTANT NOTE: YOU MUST USE YOUR OWN API KEY AND WATSONX PROJECT ID
- Open `worker.py`
- Set both `API_KEY` and `PROJECT_ID`
```cmd
ibmcloud ce application create --name personal-assistant \
  --build-source . \
  --build-context-dir . \
  --image us.icr.io/${SN_ICR_NAMESPACE}/personal-assistant:latest \
  --registry-secret icr-secret \
  --port 8000 \
  --min-scale 1 \
  --visibility project
```

Parameter explanation (continued):

- `--build-source` This specifies where the code you want to build from exists. This can be a GitHub repository - in your case use `.` which just sets the current local directory.
- `--build-context-dir` This specifies from within the build source, where to find our Docker project (again we use .)
- `--image` This is similar to above, but this time we are specifying which image we want Code Engine to create for us.

Once the app is deployed, open the URL outputted in the terminal, and enjoy your app deployed live on the web thanks to Code Engine.

##### Part 2: Deploying to your own account!
The main difference between deploying to the Skills Network Environment and your own environment is that you need to deploy the root images for the Text-to-Speech and Speech-to-Text models yourself and use the correct endpoints.

###### Step 1. Log in to your IBM Cloud account.
Using the `ibmcloud login` command log into your own IBM Cloud account. Remember to replace `USERNAME` with your IBM Cloud account email and then enter your password when prompted to.

```
ibmcloud login -u USERNAME
```

Use `ibmcloud login --sso` command to log in, if you have a federated ID.

Then target any specific resource group in your account. By default, if you've completed the sign-up process for your IBM Cloud account, you can use the `Default` resource group.

```
ibmcloud target -g Default
```

##### Step 2. Login to the IBM Entitled Registry
You'll need to log in to the IBM Entitled Registry to download the Watson Speech-to-Text and Text-to-Speech images so you can deploy them to your own Code Engine project.

Go to [IBM’s Container Library](https://myibm.ibm.com/products-services/containerlibrary?utm_source=skills_network&utm_content=in_lab_content_link&utm_id=Lab-IBMSkillsNetwork-GPXX0PPIEN) to get an Entitlement Key. This key gives you access to pulling and using the IBM Watson Speech Libraries for Embed. However, do note that this key is only valid for a Year as a trial.

Once you've obtained the Entitlement Key from the container software library you can log in to the registry with the key, and pull the images.

Replace it with your own IBM Entitlement Key.

```
IBM_ENTITLEMENT_KEY="YOUR_IBM_ENTITLEMENT_KEY"
```

Login to the docker registry to pull the images

```
echo $IBM_ENTITLEMENT_KEY | docker login -u cp --password-stdin cp.icr.io
```

###### Step 3. Build Watson Speech-to-Text image
When you cloned the original outline, it pulled a folder called models into this workspace. This folder contains the Dockerfiles for the Watson Speech-to-Text images. We will build the models using these Dockerfiles and tag them to later push them to your own registry.

```
cd /home/project/translator-with-voice-and-watsonx/models/stt
docker build ./speech-to-text -t stt-standalone:latest
```

###### Step 4. Pull and build Watson Text-to-Speech image
Similarly, build the Watson Text-to-Speech image using the Dockerfile in the models/tts directory and then tag it to later push it to your own registry.
```
cd /home/project/translator-with-voice-and-watsonx/models/tts
docker build ./text-to-speech -t tts-standalone:latest
```

###### Step 5. Create a namespace and log in to ICR
You'll need to create a namespace before you can upload your images, and make sure you're targeting the ICR region you want, which right now is global.

Choose a name for your namespace, specified as ${NAMESPACE}, and create the namespace. Currently, it's set to personal-assistant, you can choose to rename it to anything you choose.

```
NAMESPACE=personal-assistant
```
```
ibmcloud cr region-set global
ibmcloud cr namespace-add ${NAMESPACE}
ibmcloud cr login
```

###### Step 6. Push Watson images to your namespace
```
TTS_APPNAME=tts-standalone:latest
STT_APPNAME=stt-standalone:latest
```
```
REGISTRY=icr.io
# Tag and push Text-to-Speech image
docker tag ${TTS_APPNAME}:latest ${REGISTRY}/${NAMESPACE}/${TTS_APPNAME}:latest
docker push ${REGISTRY}/${NAMESPACE}/${TTS_APPNAME}:latest
# Tag and push Speech-to-Text image
docker tag ${STT_APPNAME}:latest ${REGISTRY}/${NAMESPACE}/${STT_APPNAME}:latest
docker push ${REGISTRY}/${NAMESPACE}/${STT_APPNAME}:latest
```

###### Step 7. Build and push the main app image to own registry
Set a name for your name, by default we will be using watsonx-personal-assistant

```
APP_NAME=watsonx-personal-assistant
```
```
cd /home/project/translator-with-voice-and-watsonx
# Build, Tag and push main App image
docker build . -t ${APP_NAME}:latest
docker tag ${APP_NAME}:latest ${REGISTRY}/${NAMESPACE}/${APP_NAME}:latest
docker push ${REGISTRY}/${NAMESPACE}/${APP_NAME}:latest
```

###### Step 8. Deploy the images to the Code Engine
Now we will create three applications on Code Engine to deploy our Watson and App images to them.

However, please note these commands assume you have already added a credit card to your IBM Cloud Account and will fail if one has not already been added. Though, you won't be charged until you reach a certain amount of usage. To learn more about IBM Code Engine's free tier, click here.

1: Target a region and a resource group

Choose the region closest to you and/or your target users. Picking a region closer to you or your users makes the browser extension faster. The further the region the longer the request to the model has to travel.

You can choose any region from this list:

###### Americas
- us-south - Dallas
- br-sao - Sao Paulo
- ca-tor - Toronto
- us-east - Washington DC
###### Europe
- eu-de - Frankfurt
- eu-gb - London
###### Asia Pacific
- au-syd - Sydney
- jp-tok - Tokyo

Use the following commands to target Dallas as the region and the Default resource group.

```
REGION=us-south
RESOURCE_GROUP=Default
```
```
ibmcloud target -r ${REGION} -g ${RESOURCE_GROUP}
```

2: Create and select a new Code Engine project

In this example, a project named personal-assistant will be created in the resource group set by the previous command.

```
ibmcloud ce project create --name personal-assistant
ibmcloud ce project select --name personal-assistant
```

3: Deploy Watson STT and TTS applications
```
ibmcloud ce application create \
  --name ${STT_APPNAME} \
  --port 1080 \
  --min-scale 1 --max-scale 2 \
  --cpu 2 --memory 8G \
  --image private.${REGISTRY}/${NAMESPACE}/${STT_APPNAME}:latest \
  --registry-secret ce-auto-icr-private-${REGION} \
  --visibility project \
  --env ACCEPT_LICENSE=true
```

Once the application is ready, it will output a URL in the terminal. Simply go to yourspeech_to_textfunction defined in the worker.py file, and then replace the base URL with that.

```
ibmcloud ce application create \
  --name ${TTS_APPNAME} \
  --port 1080 \
  --min-scale 1 --max-scale 2 \
  --cpu 2 --memory 8G \
  --image private.${REGISTRY}/${NAMESPACE}/${TTS_APPNAME}:latest \
  --registry-secret ce-auto-icr-private-${REGION} \
  --visibility project \
  --env ACCEPT_LICENSE=true
```

Once the application is ready, it will output a URL in the terminal. Simply go to yourtext_to_speechfunction defined in the worker.py file, and then replace the base URL with that.

4: Deploy your main app
```
ibmcloud ce application create \
  --name ${APP_NAME} \
  --port 8000 \
  --min-scale 1 --max-scale 2 \
  --cpu 1 --memory 4G \
  --image private.${REGISTRY}/${NAMESPACE}/${APP_NAME}:latest \
  --registry-secret ce-auto-icr-private-${REGION} \
  --env ACCEPT_LICENSE=true
```

It may take a few minutes to complete the deployment. If the deployment is successful, you'll get the URL of the application's public endpoint from the command output.

5: Check your deployment

You can check the status, logs and events of the application with the following commands:

```
ibmcloud ce app list
ibmcloud ce app logs --application ${APP_NAME}
ibmcloud ce app events --application ${APP_NAME}
```