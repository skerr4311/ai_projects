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
