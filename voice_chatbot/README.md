# coding-project-template

This project is cloned from the following commmand:
```cmd
git clone https://github.com/ibm-developer-skills-network/bkrva-chatapp-with-voice-and-openai-outline.git
```

#### Small prerequisites:
You need to run these commands with a single click to fulfill some of the prerequisites:
```cmd
mkdir certs/
cp /usr/local/share/ca-certificates/rootCA.crt ./certs/
```

#### 1. Starting the application
This image is quick to build as the application is quite small. These commands first build the application (running the commands in the Dockerfile) and tags (names) the built container as voice-chatapp-powered-by-openai, then runs it in the foreground on port 8000. You'll need to run these commands everytime you wish to make a new change to one of the files.
```cmd
docker build . -t voice-chatapp-powered-by-openai
docker run -p 8000:8000 voice-chatapp-powered-by-openai
```

#### 2. Starting Speech-to-Text
Skills Network provides its own Watson Speech-to-Text image that runs automatically in this environment. To access it, use this endpoint URL when you get to Step 4:

```cmd
base_url = "https://sn-watson-stt.labs.skills.network"
```

You can test it works by running this query:
```cmd
curl https://sn-watson-stt.labs.skills.network/speech-to-text/api/v1/models
```

You should see a list of a few languages it can recognize. Example output is shown below.
```json
{
   "models": [
      {
         "name": "en-US_Multimedia",
         "language": "en-US",
         "description": "US English multimedia model for broadband audio (16kHz or more)",
          ...
      },
      {
         "name": "fr-FR_Multimedia",
         "language": "fr-FR",
         "description": "French multimedia model for broadband audio (16kHz or more)",
          ...
      }
   ]
}
```

#### 3. Starting Text-to-Speech
Skills Network provides its own Watson Text-to-Speech image that is run automatically in this environment. To access it, use this endpoint URL when you get to Step 6:

```cmd
base_url = "https://sn-watson-tts.labs.skills.network"
```

You can test it works by running this query:
```cmd
curl https://sn-watson-tts.labs.skills.network/text-to-speech/api/v1/voices
```

You should see a list of a bunch of different voices this model can use. Example output is shown below.
```json
{
   "voices": [
      {
         "name": "en-US_EmilyV3Voice",
         "language": "en-US",
         "gender": "female",
         "description": "Emily: American English female voice. Dnn technology.",
         ...
      },
      {
         "name": "en-GB_JamesV3Voice",
         "language": "en-GB",
         "gender": "male",
         "description": "James: British English male voice. Dnn technology.",
         ...
      },
      {
         "name": "en-US_MichaelV3Voice",
         "language": "en-US",
         "gender": "male",
         "description": "Michael: American English male voice. Dnn technology.",
         ...
      },
      {
         "name": "fr-CA_LouiseV3Voice",
         "language": "fr-CA",
         "gender": "female",
         "description": "Louise: French Canadian female voice. Dnn technology.",
         ....
      },
      ...
   ]
}
```