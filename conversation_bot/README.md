## Conversation Bot

#### Intro: Hugging Face
Hugging Face is an organization that focuses on natural language processing (NLP) and AI. They provide a variety of tools, resources, and services to support NLP tasks.

You'll be making use of their Python library transformersin this project.

Alright! Now that you know how a chatbot works at a high level, let's get started with implementing a simple chatbot!

#### Step 1: Installing requirements
Follow these steps to create a Python virtual environment and install the necessary libraries. Open a new terminal first.
Set up your virtual environment:
```cmd
pip3 install virtualenv 
virtualenv my_env # create a virtual environment my_env
source my_env/bin/activate # activate my_env
```

For this example, you will be using the transformers library, which is an open-source natural language processing (NLP) toolkit with many useful features, and also let's install a torch library.

```cmd
python3 -m pip install transformers==4.30.2 torch
```

Wait a few minutes to install the packages.

#### Chosing a model
Choosing the right model for your purposes is an important part of building chatbots! You can read on the different types of models available on the Hugging Face website: [https://huggingface.co/models](https://huggingface.co/models).

LLMs differ from each other in how they are trained. Let's look at some examples to see how different models fit better in various contexts.

##### Text generation:
If you need a general-purpose text generation model, consider using the GPT-2 or GPT-3 models. They are known for their impressive language generation capabilities.
Example: You want to build a chatbot that generates creative and coherent responses to user input.

##### Sentiment analysis:
For sentiment analysis tasks, models like BERT or RoBERTa are popular choices. They are trained to understand the sentiment and emotional tone of text.
Example: You want to analyze customer feedback and determine whether it is positive or negative.

##### Named entity recognition:
LLMs such as BERT, GPT-2, or RoBERTa can be used for Named Entity Recognition (NER) tasks. They perform well in understanding and extracting entities like person names, locations, organizations, etc.
Example: You want to build a system that extracts names of people and places from a given text.

##### Question answering:
Models like BERT, GPT-2, or XLNet can be effective for question-answering tasks. They can comprehend questions and provide accurate answers based on the given context.
Example: You want to build a chatbot that can answer factual questions from a given set of documents.

##### Language translation:
For language translation tasks, you can consider models like MarianMT or T5. They are designed specifically for translating text between different languages.
Example: You want to build a language translation tool that translates English text to French.

However, these examples are very limited and the fit of an LLM may depend on many factors such as data availability, performance requirements, resource constraints, and domain-specific considerations. It's important to explore different LLMs thoroughly and experiment with them to find the best match for your specific application.

Other important purposes that should be taken into consideration when choosing an LLM include (but are not limited to):

Licensing: Ensure you are allowed to use your chosen model the way you intend
Model size: Larger models may be more accurate, but might also come at the cost of greater resource requirements
Training data: Ensure that the model's training data aligns with the domain or context you intend to use the LLM for
Performance and accuracy: Consider factors like accuracy, runtime, or any other metrics that are important for your specific use case
To explore all the different options, check out the available models on the [Hugging Face website](https://huggingface.co/models?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkGuidedProjectsIBMSkillsNetworkGPXX04ESEN3232-2023-01-01).

For this example, you'll be using ```facebook/blenderbot-400M-distill``` because it has an open-source license and runs relatively fast.

```cmd
model_name = "facebook/blenderbot-400M-distill"
```