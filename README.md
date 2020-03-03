## !! This project is a work in progress.

# Set up [RASA](https://rasa.com/) chatbot using [Rasa UI](https://github.com/paschmann/rasa-ui) and [webchat](https://github.com/botfront/rasa-webchat)

**Youtube playlist** \
[https://www.youtube.com/playlist?list=PL75e0qA87dlHQny7z43NduZHPo6qd-cRc](https://www.youtube.com/playlist?list=PL75e0qA87dlHQny7z43NduZHPo6qd-cRc)

**github link for RASA Master Class Demo** \
[https://github.com/RasaHQ/rasa-masterclass](https://github.com/RasaHQ/rasa-masterclass)

**Installation of RASA** \
[https://rasa.com/docs/rasa/user-guide/installation/](https://rasa.com/docs/rasa/user-guide/installation/)  
please refer above URL for Installation of RASA, Python and other dependency

# Setup Using Docker-Compose
[https://rasa.com/docs/rasa/user-guide/running-rasa-with-docker/](https://rasa.com/docs/rasa/user-guide/running-rasa-with-docker/) \
Install Docker and keep directory structure as below for docker compose.

```language
├── app
├── html
│   └── index.html
├── rasa-ui
│   └── server
│         └── data
│               └── .gitignore
│               └──  models
│                      └── .gitignore
├── docker-compose.yml
```

now run below command to up docker containers

```bash
docker-compose up -d
```

then connect to rasa container using below command 

```bash
docker exec -it  <rasa container id> bash
```

If you are upping docker container first time then there will not any file in **‘/app/’** folder for training data, to create training data files you have to initialise new project, if already initialised then only training data file will create in **‘/app/’** folder

```bash
rasa init --no-prompt
```

:warning: **Note - above command will overwrite all training data file with existing files if there any**

below files will crate in **/app/** folder and will Training an initial model

<table>
    <tr>
        <td>
            __init__.py
        </td>
        <td>
            an empty file that helps python find your actions
        </td>
    </tr>
    <tr>
        <td>
            actions.py
        </td>
        <td>
            code for your custom actions
        </td>
    </tr>
    <tr>
        <td>config.yml ‘*’
        </td>
        <td>configuration of your NLU and Core models
        </td>
    </tr>
    <tr>
        <td>credentials.yml
        </td>
        <td>details for connecting to other services
        </td>
    </tr>
    <tr>
        <td>data/nlu.md ‘*’
        </td>
        <td>your NLU training data
        </td>
    </tr>
    <tr>
        <td>data/stories.md ‘*’
        </td>
        <td>your stories
        </td>
    </tr>
    <tr>
        <td>domain.yml ‘*’
        </td>
        <td>your assistant’s domain
        </td>
    </tr>
    <tr>
        <td>endpoints.yml
        </td>
        <td>details for connecting to channels like fb messenger
        </td>
    </tr>
    <tr>
        <td>models/&lt;timestamp&gt;.tar.gz
        </td>
        <td>your initial model
        </td>
    </tr>
</table>
<br>
To set newly trained model, you need to down docker-compose using below command

```bash
docker-compose down
```


<br>

### Rasa Webchat
Use [Rasa webchat](https://github.com/botfront/rasa-webchat) widget to connect with a chatbot 💬 platform. \
Create index.html in **/html/** folder and paste below code on body tag.

```html
<div id="webchat"/>
<script src="https://storage.googleapis.com/mrbot-cdn/webchat-latest.js"></script>
// Or you can replace latest with a specific version
<script>
  WebChat.default.init({
    selector: "#webchat",
    initPayload: "/get_started",
    customData: {"language": "en"},
    socketUrl: "http://localhost:5500",
    socketPath: "/socket.io/",
    title: "Title",
    subtitle: "Subtitle",
  })
</script>
```

To set backend connection between Rasa Webchat and Rasa server, below code in **/app/credentials.yml** file.

```yml
socketio:
  user_message_evt: user_uttered
  bot_message_evt: bot_uttered
  session_persistence: true
```

Now again up docker-compose using below command

```bash
docker-compose up -d
```

Now you can test your new created chatbot on [http://localhost:8100/](http://localhost:8100/) url
<br><br><br>

### Train NLU model using RASA server

You can train your NLU model by editing training data file from **/app/ folder** 
<br><br>
After updates in  stories and NLU data, connect to rasa server and run command to train model.

```bash
docker exec -it  <rasa container id> bash
rasa train
```

This command will the trained model and save trained model into the **/app/models/** directory.
<br><br>
Whenever you trained model, you have to down the docker containers and again have to up the docker containers using the below command to make active newly trained model in production.

```bash
docker-compose down
```

then

```bash
docker-compose up -d
```

<br><br>

### Train NLU model using [Rasa UI](https://github.com/paschmann/rasa-ui) server

You can train NLU model using Rasa UI web application. for more detail about Rasa UI check below link \
[https://github.com/paschmann/rasa-ui](https://github.com/paschmann/rasa-ui)

You can connect Rasa UI web application on [http://localhost:5001](http://localhost:5001) \
As per Rasa UI username is "admin" and password is "admin". but you can change this by editing environment variable from docker-compose.yml file. 
Now as per docker-compose.yml file login details are below \
**username - admin** \
**password - 321**

Use the below command to connect Rasa UI container

```language
docker exec -it <rasa_ui container id> sh
```

Below are step to train NLU Model.

**Create a New Bot** \
Connect Rasa UI web application on [http://localhost:5001](http://localhost:5001) and select tab "Bots" from left side panel, click on right top (+) button to add a new bot. \
![example1](/shailendra/rasa-paschmann-rasa-ui-docker-compose/raw/master/raw/dashboard.jpg)


