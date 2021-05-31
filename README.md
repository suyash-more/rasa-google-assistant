# *Building a Rasa-powered Google Assistant*

***
![img](rasa2.png)
***
This repository contains the code of the tutorial of connecting Rasa-powered assistant to Google Assistant. You can find the step-by-step tutorial here.

***

## Repository file Info?

This repository consists of the following files and directories:
>
>- **place_finder** - a directory which contains a pre-built Rasa assistant called Place Finder. This assistant is used in this tutorial to demonstrate the integration to Google Assistant.
>- **action.json** - a custom Google Assistant action configuration file.
>- **ga_connector.py** - a custom Rasa-Google Assistant connector. If you follow the tutorial using your own assistant, add this connector to your project directory.


## Follow the process now : 

- First create your own virtual env
If your OS is linux :
```sh
python3 -m venv venv
source venv/bin/activate
```
If your OS is Windows :
```sh
python -m venv venv
venv\Scripts\activate
```

- Install all the dependencies

```sh
pip install -r requirements.txt
```

- Tools/utilities we have used in this project are

>[Google Actions console](https://console.actions.google.com/) <br>
>[Rasa](https://github.com/RasaHQ/rasa) <br>
>[Ngrok](https://ngrok.com/) <br>
>[Google gactions CLI](https://developers.google.com/actions/tools/gactions-cli)
***
**Google Actions console** will be used to create a assistant which will take 
inputs from the user and provide it to our project

**Rasa** is the main  brain of our chatbot as in it will give us the answers to 
all the queries performed by the user

**Ngrok** it is used for making a tunnel to expose a locally 
running application to the outside world

**Google gactions CLI** will be used to push all the actions and from 
where the assistant needs to pickup the conversation replies i.e. the urls
is specified in the **actions.json** file which is created by gactions
In this case you need to download the CLI and keep it in your main project folder
***
Put you API key of the project in credentials.yml file
***
After your installations are done

#### Step 1 :
Train your model with the following command which will run train both NLU and dialogue models and 
save them as a compressed file in a models directory
```sh
rasa train
```

#### Step 2 :
- Create a project at console actions and add Actions SDK if available to your project
Elsewhere create a custom blank project
- Set your display name and voice of yur bot


#### Step 3 :
Go to your main project directory where gactions.exe is kept.
Then run (remember your gactions must have +x[executable] permissions)
```sh
gactions init
```

- Fill in your actions and conversations and the url which at which locally your project will be running

In order to get the url field in the actions.json
1. Run the http server of ngrok at 5004 (port as per your choice)
2. It will provide you a https url which will be kind of  
```shell
https://131c47488d0e.ngrok.io
```
3. The final url will be 
```sh
    https://131c47488d0e.ngrok.io/webhooks/google_assistant/webhook
```

#### Step 4 :

Now we will write a custom connector to connect Rasa to Google Assistant.
- Watch out the route names where the **GET** and **POST** requests are performed and the fetching of information also
- We will need to put in the url in order to what we have mentioned the url there
- So by now your webhook is ready to accept/send responses


#### Step 5 :

- All that is left to do is to start the Rasa server which will 
use custom connector to connect to Google Assistant.
So run the following command : 
```sh
rasa run --enable-api -p 5004
```

- Rasa assistant includes custom actions (just like the Place Finder does), make sure to start the 
actions server by running the following command:
```sh
rasa run actions
```  

- If you have DucklingHTTPExtractor in your NLP pipeline make sure to start a local server 
  for it as well. Make sure Docker is installed. You can do it by running a command below:
```sh
docker run -p 8000:8000 rasa/duckling
```  

- Now, all that is left to do is to deploy your custom Google Assistant skill and enable testing. 
You can deploy the custom Google Assistant skill by running the command 
below. Here, PROJECT_ID is a value of the --project parameter
You will need your project ID which you will find in the 
project setting once the project is created
  
```sh
gactions update --action_package action.json --project PROJECT_ID
```

- Once the action is uploaded, enable testing of the action 
by running the following command:

```sh
gactions test --action_package action.json --project PROJECT_ID
```
***

#### *Kudos..!! You have just built a custom Google Assistant skill with Rasa Stack!*