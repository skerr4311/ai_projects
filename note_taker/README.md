## Business AI Meeting Companion STT

#### Preparing the environment
Let's start with setting up the environment by creating a Python virtual environment and installing the required libraries, using the following commands in the terminal:
```env
pip3 install virtualenv 
virtualenv my_env # create a virtual environment my_env
source my_env/bin/activate # activate my_env
```

Then, install the required libraries in the environment (this will take time ☕️☕️):

##### installing required libraries in my_env
```env
pip install transformers==4.36.0 torch==2.1.1 gradio==5.23.2 langchain==0.0.343 ibm_watson_machine_learning==1.0.335 huggingface-hub==0.28.1
```

Have a cup of coffee, it will take a few minutes.
```env
      )  (
     (   ) )
      ) ( (
    _______)_
 .-'---------|  
( C|/\/\/\/\/|
 '-./\/\/\/\/|
   '_________'
    '-------'
```
We need to install ffmpeg to be able to work with audio files in python.

```env
sudo apt update
```
Then run:
```env
sudo apt install ffmpeg -y
```