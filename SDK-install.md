# Serverless Application Design and Implementation Using Raspberry Pi and AWS

## Installing AWS SDKs on the hosts
We need to install the AWS SDKs on both the Windows 10 and the Rpi hosts. We are installing the following components:

| Service | Usage |
| --- | --- |
| AWS CLI | Allows us access to lower level API calls |
| ASK CLI | This is the Alexa Skill Kit API |
| Boto3 | The is main API for all the cloud services. The API has a layer of abstraction over the CLI API |

## Install AWS CLI on hosts
Use the following instructions to [Install AWS CLI Version 1](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html) on both the Raspberry Pi and the Windows 10 hosts. Both version 1 and 2 are available. We are going to use version 1 in this project. After you have installed the SDK type the following in a console session on both machines: `aws --version`. You should see something like:
```
C:\Users\rmongeon>aws --version
aws-cli/1.16.292 Python/3.6.0 Windows/10 botocore/1.13.28
```

and on the Rpi it looks like this:
```
pi@raspberrypi:~ $ aws --version
aws-cli/1.16.309 Python/3.7.3 Linux/4.19.75-v7l+ botocore/1.13.45
```


## Install the AWS Alexa Skills Kit (ASK) API
[Install AWS ASK CLI](https://developer.amazon.com/en-US/docs/alexa/smapi/quick-start-alexa-skills-kit-command-line-interface.html) on both the Rpi and Windows hosts. You will need to install Node.js in order to do this.


## Install Boto3
[Install Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html) on both the Rpi and the Win10 box.


## Setup an AWS IAM account 
Use the [following guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) to setup and administrative user. Call the user "awsprog".


## Initializing the AWS CLI
Now we need to setup the default AWS security for the AWS CLI. Use the [following guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) to setup the AWS CLI. When you have completed all the above actions you should be able to type the following in the command line on both the Rpi and the Win10 box:
    `aws iam get-user`
and both machines should return:

```json
{
    "User": {
        "Path": "/",
        "UserName": "awsprog",
        "UserId": "AIDAZ6BQVJ7BBB2KFJKLK",
        "Arn": "arn:aws:iam::683001800000:user/awsprog",
        "CreateDate": "2019-12-31T00:59:05+00:00",
        "Tags": [
            {
                "Key": "pythonadmin",
                "Value": "This is a Python Program user with full access"
            }
        ]
    }
}
```

## Python Development:
One thing you might be noticing is that I am not using virtual environments in this project. This is intentional as I didn't want to add another new topic to the project.




