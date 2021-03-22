# Restaurant-Chatbot
Restaurant chatbot developed using RASA NLU and python 3.
## Pre-requisites

- python 3.7.0
- rasa 1.1.4
- spacy 2.1.4
- en_core_web_md 2.1.0

## Installation

### RASA

Refer to official [installation guide](https://rasa.com/docs/rasa/user-guide/installation/) to install RASA

# Installing Rasa and Rasa X:

You can install both Rasa and Rasa X using the following code:

$ pip install rasa-x --extra-index-url https://pypi.rasa.com/simple

Use the following code only if you want to install Rasa:

$ pip install rasa

Install Rasa NLU and Spacy in the same command prompt:

$ pip install rasa[spacy]

$ python -m spacy download en_core_web_md

$ python -m spacy link en_core_web_md en

### Data Files

- **data/nlu/nlu.md** : contains training examples for the NLU model  
- **data/core/stories.md** : contains training stories for the Core model  

### Script Files

- **zomato** : contains Zomato API integration code
  - **zomato_api.py** : contains functions to consume common Zomato APIs like fetch location details, type of cuisines, search for restaurants, etc
  - **zomato_test.py** : contains 3 test functions for Zomato API - location details, cuisine details and restaurant search
- **action_api_test.py** : executes Zomato API test functions from command line
- **actions_server.py** : contains code to start a RASA actions server. This is used to serve RASA custom actions.
- **actions.py** : contains the following custom actions (_insert **ZOMATO API** key in this script file before starting RASA server_)
  - search restaurant
  - validate location
  - validate cuisine
  - send email
  - restart conversation
  - reset slots  
  - train a NLU model
  - persist NLU model in 'models' folder
  - run CLI to interact with generated model and validate extracted intents and entities
- **rasa_slack.py** : contains code to integrate with slack channel
- **rasa_train.py** : primary script file to test chatbot from CLI. Contains code to:
  - train both NLU and Core model
  - persist packaged model in 'models' folder
  - start CLI interface to interact with chatbot
    (_RASA action server must be running for this to work_)

### Config Files

- **config.yml** contains model configuration and custom policy
- **credentials.yml** contains authentication token to connect with channels like slack
- **domain.yml** defines chatbot domain like entities, actions, templates, slots  
- **endpoints.yml** contains the webhook configuration for custom action
- **smtpconfig.txt** contains SMTP server configuration information (_If using GMAIL, setup account and generate app password_) .

## Usages

- Train ONLY NLU model and validate

 Rasa train nlu

  This will generate _restaurant-nlu-model.tar.gz_ inside **models** folder and start an interactive shell with command rasa shell nlu

  ```

- Run RASA action server ( mandatory step to interact with chatbot) on port **5055**

  ```
  Equivalent RASA CLI command 
  
  ```python
  rasa run actions
  ```
  
- Train RASA NLU and Core model


  ```rasa train core

  This will generate _restaurant-rasa-model.tar.gz_ inside **models** folder

  Equivalent RASA CLI command 
  
  ```python
  rasa train
  ```

- Starts an interactive session with restaurant chatbot

  ```python
  python rasa_train.py --shell
  ```

  _This step will recreate RASA NLU and Core models_

  Equivalent RASA CLI command  
  
  ```python
  rasa shell
  ```

- Run RASA server to connect slack channel (Optional)

  ```python
  rasa run -m models -p 5004 --connctor slack --credentials credentials.yml
  ```
---

Problem Statement

An Indian startup named 'Foodie' wants to build a conversational bot (chatbot) which can help users discover restaurants across several Indian cities. You have been hired as the lead data scientist for creating this product.

The main purpose of the bot is to help users discover restaurants quickly and efficiently and to provide a good restaurant discovery experience. The project brief provided to you is as follows.

The bot takes the following inputs from the user:

City: Take the input from the customer as a text field. For example:

Bot: In which city are you looking for restaurants?

User: anywhere in Delhi

Important Notes: 

Assume that Foodie works only in Tier-1 and Tier-2 cities. You can use the current HRA classification of the cities from here. Under the section 'current classification' on this page, the table categorizes cities as X, Y and Z. Consider 'X ' cities as tier-1 and 'Y' as tier-2. 
The bot should be able to identify common synonyms of city names, such as Bangalore/Bengaluru, Mumbai/Bombay etc.
 

Your chatbot should provide results for tier-1 and tier-2 cities only, while for tier-3 cities, it should reply back with something like "We do not operate in that area yet".

Cuisine Preference: Take the cuisine preference from the customer. The bot should list out the following six cuisine categories (Chinese, Mexican, Italian, American, South Indian & North Indian) and the customer can select any one out of that. Following is an example for the same:

Bot: What kind of cuisine would you prefer?

Chinese
Mexican
Italian
American
South Indian
North Indian
User: I’ll prefer Italian!


Average budget for two people: Segment the price range (average budget for two people) into three price categories: lesser than 300, 300 to 700 and more than 700. The bot should ask the user to select one of the three price categories. For example:

Bot: What price range are you looking at?

Lesser than Rs. 300
Rs. 300 to 700
More than 700
User: in range of 300 to 700

While showing the results to the user, the bot should display the top 5 restaurants in a sorted order (descending) of the average Zomato user rating (on a scale of 1-5, 5 being the highest). The format should be: {restaurant_name} in {restaurant_address} has been rated {rating}.


Finally, the bot should ask the user whether he/she wants the details of the top 10 restaurants on email. If the user replies 'yes', the bot should ask for user’s email id and then send it over email. Else, just reply with a 'goodbye' message. The mail should have the following details for each restaurant:

Restaurant Name
Restaurant locality address
Average budget for two people
Zomato user rating
You can refer to the following links on how to send emails through Python:

Python Email Package
Python Flask Mail
(A heads up: You'll have to enable secure access on Gmail to allow Python to send emails).

 

You have been given some sample conversational stories in the ‘test’ file. Look at the stories and try to observe entities, intents, dialogue-flow which may be useful to train the NLU and also the dialogue flow.
 

Goals of this Project
In this project, you will build a chatbot for ‘Foodie’ and then deploy it on Slack. The folder with starter codes has already been shared with you in session-1. You need to accomplish the following in the project:

NLU training: You can use rasa-nlu-trainer to create more training examples for entities and intents. Try using regex features and synonyms for extracting entities.

Build actions for the bot. Read through the Zomato API documentation to extract the features such as the average price for two people and restaurant’s user rating. You also need to build an ‘action’ for sending emails from Python.

Creating more stories: Use train_online.py file to create more stories. Refer to the sample conversational stories provided above.  Your bot will be evaluated on something similar to the test stories shared.

Deployment (Optional): Deploy the model on Slack. You can create a new workspace or deploy it on an existing workspace (if you already use Slack).


