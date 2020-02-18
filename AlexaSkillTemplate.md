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

# TODO: ~
Implement the Lambda function 
 



