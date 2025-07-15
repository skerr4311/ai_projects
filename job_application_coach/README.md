### Setting up a virtual environment
Let's create a virtual environment. Using a virtual environment allows you to manage dependencies for different projects separately, avoiding conflicts between package versions.

In the terminal of your Cloud IDE, ensure that you are in the path /home/project, then run the following commands to create a Python virtual environment.

```
pip install virtualenv 
virtualenv my_env # create a virtual environment named my_env
source my_env/bin/activate # activate my_env
```

##### Installing necessary libraries
To ensure a seamless execution of our scripts, and considering that certain functions within these scripts rely on external libraries, it's essential to install some prerequisite libraries before we begin. For this project, the key libraries we’ll need are Gradio for creating user-friendly web interfaces and IBM Watson Machine Learning for leveraging advanced LLM model.

Gradio package: Gradio allows us to build interactive web applications quickly, making our AI models accessible to users with ease.
IBM Watson Machine Learning package: IBM Watson Machine Learning package integrates powerful IBM LLM models into our project.
Here's how to install these packages (still from your terminal):
```
# installing necessary pacakges in my_env
python3.11 -m pip install gradio==5.12.0 
python3.11 -m pip install ibm_watsonx_ai==1.1.20
python3.11 -m pip install email-validator==2.1.1 
python3.11 -m pip install numpy==1.26.4 
python3.11 -m pip install pandas==2.1.4
```

Now, the environment is ready to create Python files.

### Use LLMs with watsonx.ai Locally
The `ibm_watsonx_ai` Python library allows you to work with IBM watsonx Machine Learning services, including LLMs. You can train, store, and deploy your models. You can also evaluate them using APIs. Additionally, it offers access to pre-trained, state-of-the-art LLMs for utilization via APIs, seamlessly integrating them into your application development process. Here is an introduction [documentation](https://ibm.github.io/watson-machine-learning-sdk/index.html).

```
pip install ibm-watson-machine-learning 
# you can also specifiy the package version such as 1.1.20
```

For example, the following is a code snippet for creating a simple QA chat with Llama2 using watsonx.ai's machine learning library.

```py
from ibm_watsonx_ai import Credentials
from ibm_watsonx_ai import APIClient
from ibm_watsonx_ai.foundation_models import Model, ModelInference
from ibm_watsonx_ai.foundation_models.schema import TextChatParameters
# Set up the API key and project ID for IBM Watson 
watsonx_API = "" # below is the instruction how to get them
project_id= "" # like "0blahblah-000-9999-blah-99bla0hblah0"
# Generation parameters
params = TextChatParameters(
    temperature=0.7,
    max_tokens=1024
)
model = ModelInference(
    model_id='meta-llama/llama-3-2-11b-vision-instruct', 
    params=params,
    credentials={
        "apikey": watsonx_API,
        "url": "https://us-south.ml.cloud.ibm.com"
    },
    project_id=project_id
    )
q = "How to be happy?"
messages = [
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": q
                },
            ]
        }
    ]
generated_response = model.chat(messages=messages)
print(generated_response['choices'][0]['message']['content'])
```

As you might have noticed, you need to have `watsonx_API` and `project_id` for authentication when using the watsonx API. The following instructions will guide you how to get them.

## Get your Own API key and project ID
#### watsonx API
You can get the `watsonx_API` key by first signing up for IBM Cloud at [IBM watsonx account](https://dataplatform.cloud.ibm.com/registration/stepone?utm_source=skills_network&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Medium-WatsonXBlog-2023-09-15&context=wx&apps=all%3Futm_source%3Dskills_network&utm_term=10006555&utm_medium=in_blog_link&utm_source_platform=medium).

After you sign up and sign in to IBM watsonx account, you can follow this demonstration to create/get your IBM Cloud user API key at https://cloud.ibm.com/iam/apikeys.

#### Project ID
Next, you need to create/get a project ID for your [watsonx.ai](https://dataplatform.cloud.ibm.com/registration/stepone?utm_source=skills_network&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Medium-WatsonXBlog-2023-09-15&context=wx&apps=all%3Futm_source%3Dskills_network&utm_term=10006555&utm_medium=in_blog_link&utm_source_platform=medium) project. Go into your project on watsonx.ai and create a project:

In the management console, find the `project_id`.

You can see the project id in:
Management –> General
(⚠️ Wait! ⚠️ it is NOT done yet!). You also need to add a service to your project to make it work.

To add a service to your project (as the image below shows), after you create a project:

⇨ Go to the project's Manage tab.
⇨ Select the Service and integrations page.
⇨ Click Associate Services.
⇨ Add the Watson Machine Learning service to it.

Alt text


🎉 Done! 🎉
Now, you can input the `watsonx_API` and `project_id` into the code snippet and try it. The sample output should be similar to this: