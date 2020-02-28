# Set up RASA chatbot using Rasa UI and webchat

**Youtube playlist** \
[https://www.youtube.com/playlist?list=PL75e0qA87dlHQny7z43NduZHPo6qd-cRc](https://link)

**github link for RASA Master Class Demo** \
[https://github.com/RasaHQ/rasa-masterclass](https://link)

**Installation of RASA** \
[https://rasa.com/docs/rasa/user-guide/installation/](https://link)  
please refer above URL for Installation of RASA, Python and other dependency

# Setup Using Docker-Compose
[https://rasa.com/docs/rasa/user-guide/running-rasa-with-docker/](https://link) \
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

If you are upping docker container first time then there will not any file in **‘/app/’** folder for training data, 
to create training data files you have to initialise new project, if already initialised then only training data file will create in **‘/app/’** folder

```bash
rasa init --no-prompt
```

**Note - above command will overwrite all training data file with existing files if there any**

below files will crate in **/app/** folder

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