# Serverless Application Design and Implementation Using Raspberry Pi and AWS

## Setup ASK SDK
We are going setup our Win10 host setup for Alexa skill development. Use the [following instructions](https://developer.amazon.com/en-US/docs/alexa/alexa-skills-kit-sdk-for-python/set-up-the-sdk.html) 

## Setup ASK SDK on Python Virtual Development
Since the majority of the instructions I have seen for Alexa Skill Dev is based on using Python Vitrual Environments we are going to use that technique for development on the Win10 host. Follow [this guide](https://developer.amazon.com/en-US/docs/alexa/alexa-skills-kit-sdk-for-python/set-up-the-sdk.html#set-up-sdk-in-virtual-environment) to set up the virtual environment.

## Python Virtual Development with Win10
All of the documentation I have seen so far for a fast and easy Python Alexa skill demo on Windows goes about showing examples of using a Virtual Environment in Linux. I'm now going to demystify that for Win10. Hold on, follow this: 

```
F:\AWS>md skill
F:\AWS\cd skill
F:\AWS\skill\virtualenv skill_env

Using base prefix 'c:\\program files\\python37'
New python executable in F:\AWS\skill\skill_env\Scripts\python.exe
Installing setuptools, pip, wheel...
done.

F:\AWS\skill>cd skill_env\Scripts
F:\AWS\skill_env>activate
(skill_env) F:\AWS\skill\skill_env\Scripts>
(skill_env) F:\AWS\skill\skill_env\Scripts>pip install ask-sdk

... 5 minutes later this completes.. enter the following to see your installed modules
(skill_env) F:\AWS\skill\skill_env\Scripts>pip list
You should see something like:
...

Package                              Version
------------------------------------ ----------
ask-sdk                              1.13.0
ask-sdk-core                         1.13.0
ask-sdk-dynamodb-persistence-adapter 1.13.0
ask-sdk-model                        1.20.2
ask-sdk-runtime                      1.13.0
boto3                                1.11.16
botocore                             1.14.16
...
```
So now we have all of our depandent modules. The actual location of the place we need place our code in is called `site-packages` directory and for us that location is:
```
    (skill_env) F:\AWS\skill\skill_env\Lib\site-packages>
```    

## Implement Hello World
Use the following instructions to [Implement Hello World](https://developer.amazon.com/en-US/docs/alexa/alexa-skills-kit-sdk-for-python/develop-your-first-skill.html).




