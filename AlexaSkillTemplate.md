# Serverless Application Design and Implementation Using Raspberry Pi and AWS

## Create an AWS hosted Alaxa Skill
I wanted to create a basic boilerplate template skill just to add to any project. We are going to use the default Python
libraries that are installed in the cloud. My goal here is that I want to be able to quickly edit the code in the Lambda text editor 
to actually quickly develop and test the code. The other option is to use a fully blown out Python Virtual Environment that requires more than just editing the code and hitting save.

### Create a new Alexa Skill from scratch

- Step 1: Log into the [Alexa Developer Console](https://developer.amazon.com/alexa/console/ask)
- Step 2: Click `Create Skill`
- Step 3: Name the skill `test harness`, make sure `custom` model is selected, make sure `Alexa-Hosted (Python)` is selected and click `Create Skill` in the top right corner.
- Step 4: Select `Hello World Skill` and click `Choose` in the right corner.
- Step 5: You should have gotton to a Build page that built out the skill. In the left toolbar click, `Invocation` and notice that your skill will be invoked when the "utterance" test harness is spoken.
- Step 6: Create a custom event by clicking `+ Add` in the intents left toolbar.
- Step 7: Name the custom intent `getData`
- Step 8: In the left toolbar under `Intents` select `getData` and you'll be asked to enter a few Sample Utterances.
- Step 9: Enter a few utterances into the intent. Enter:
    `get data` and click `the + sign` to add the utternace. Add `get latest data`, `sensor data` and `latest data` as well.
- Step 10: Click `Save Model` and then `Build Model` and wait until you get a successful build.
- Step 11: Test the skill's voice user interface. In the top right corner click the `Evaluate Model` button.
- Step 12: In the `Utterance Profiler` enter `get sensor data` and click `Submit`
- Step 13: Insure that you utterance was mapped to the `getData intent`. We will need our skill to implement this event.
- Step 14: Click on `JSON Editor` and note the JSON code should look similar to what I have below:

```
{
    "interactionModel": {
        "languageModel": {
            "invocationName": "test harness",
            "intents": [
                {
                    "name": "AMAZON.CancelIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.HelpIntent",
                    "samples": []
                },
                {
                    "name": "AMAZON.StopIntent",
                    "samples": []
                },
                {
                    "name": "HelloWorldIntent",
                    "slots": [],
                    "samples": [
                        "hello"
                    ]
                },
                {
                    "name": "AMAZON.NavigateHomeIntent",
                    "samples": []
                },
                {
                    "name": "getData",
                    "slots": [],
                    "samples": [
                        "sensor data",
                        "latest data",
                        "get latest data"
                    ]
                }
            ],
            "types": []
        }
    }
}
```
## Create the Lambda code
We are creating some custom code for the Lambda back end. The goal is to be able to edit and debug on the fly so as long as we only use the default libraries we should be alright.

# Here is the code framework I've been working with:

```


from botocore.vendored import requests
import collections
import boto3
import time
import json
import urllib3

from boto3.dynamodb.conditions import Key, Attr

def GetWelcomeMessage():
    return "You can say Get Sensor Data or Get Last Data"

##############################
# Builders
##############################
def build_PlainSpeech(body):
    speech = {}
    speech['type'] = 'PlainText'
    speech['text'] = body
    return speech

def build_response(message, session_attributes={}):
    response = {}
    response['version'] = '1.0'
    response['sessionAttributes'] = session_attributes
    response['response'] = message
    return response

def build_SimpleCard(title, body):
    card = {}
    card['type'] = 'Simple'
    card['title'] = title
    card['content'] = body
    return card

##############################
# Responses
##############################
def conversation(title, body, session_attributes):
    speechlet = {}
    speechlet['outputSpeech'] = build_PlainSpeech(body)
    speechlet['card'] = build_SimpleCard(title, body)
    speechlet['shouldEndSession'] = False
    return build_response(speechlet, session_attributes=session_attributes)

def statement(title, body):
    speechlet = {}
    speechlet['outputSpeech'] = build_PlainSpeech(body)
    speechlet['card'] = build_SimpleCard(title, body)
    speechlet['shouldEndSession'] = True
    return build_response(speechlet)

def continue_dialog():
    message = {}
    message['shouldEndSession'] = False
    message['directives'] = [{'type': 'Dialog.Delegate'}]
    return build_response(message)

# Say a sentence but don't end the conversation
def statementKeepSession(title, body, session_attributes):
    speechlet = {}
    speechlet['outputSpeech'] = build_PlainSpeech(body)
    speechlet['card'] = build_SimpleCard(title, body)
    speechlet['shouldEndSession'] = False
    return build_response(speechlet, session_attributes=session_attributes)

##############################
# Custom Intents
##############################
def GetSensorData_intent(event, context):
    strCN = GetWelcomeMessage()
    return statement("Template", strCN)

##############################
# Required Intents
##############################
def cancel_intent():
    return statement("CancelIntent", "You want to cancel")	#don't use CancelIntent as title it causes code reference error during certification 

def help_intent():
    return statement("CancelIntent", "You want help")		#same here don't use CancelIntent

def stop_intent():
    return statement("StopIntent", "You want to stop")		#here also don't use StopIntent

# this event gets called when the user switches the application. it was defined in 
# the docs after I started learning
# it wasn't handled in the example I saw and was adding frustration to learning
def fallback_intent():
    return statement("FallbackIntent", "FallBack intent was invoked, you have ended the session")

##############################
# On Launch
##############################
def on_launch(event, context):
    # strTest = " on launch succeeded "
    # strQuery = GetTempInfo()
    strResp = GetWelcomeMessage()
    return statement("Greetings", "Welcome to the Messege queue. " + strResp)
    
def on_launchNotHandled(event, context):
    str = event['request']['type']
    return statement("Unhandled", "event type is unhandled for request type, " + str)


##############################
# Routing: Here we are mapping VUI intents to code
##############################
def intent_router(event, context):
    intent = event['request']['intent']['name']
    szErr = "Inside intent router looking for intent, " + intent

    # Custom Intents
    if intent == "getSensorData":
        return GetSensorData_intent(event, context)
        
    # Required Intents
    if intent == "AMAZON.CancelIntent":
        return cancel_intent()

    if intent == "AMAZON.HelpIntent":
        return help_intent()

    if intent == "AMAZON.StopIntent":
        return stop_intent()
        
    if intent == "AMAZON.FallbackIntent":
        return fallback_intent()

    return szErr
# end intent router


##############################
# Program Entry
##############################
def lambda_handler(event, context):
    if event['request']['type'] == "LaunchRequest":
        return on_launch(event, context)

    elif event['request']['type'] == "IntentRequest":
        return intent_router(event, context)
    else:
        return on_launchNotHandled(event, context)

```

## Next up we are going to test the function above:

