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