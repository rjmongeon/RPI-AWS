# Serverless Application Design and Implementation Using Raspberry Pi and AWS
## Project Use Case

## General Idea of the project:
Essentially what we are going to do in this project is build out a small microcomputer called a Raspberry Pi (Rpi) for use as a sensor and actuator. It's what Amazon calls an "IoT Thing". The Rpi will send real time sensor data to an outgoing software queue and at the same time monitor an incoming software queue for commands it needs to perform such as turn on or off the heat and humidity. After the Rpi is built then we will install the AWS Python SDKs and write the code to interface the device with the cloud. This project touches on many technologies you should be familiar with if your goal is to get it working quickly. 

## Problem Statement:
The Production department needs to be able to monitor and control the humidity and temperature of a “clean room” producing vegetable produce. The current process is manual involving employees to physically look at gauges, write log entries and adjust a humidifier and thermostat on an hourly schedule. The Production department wants to automate the logging process but still allow for an operator to manually control the heat and humidity through a user interface.   

## The author's personal goals:
I have many years experience writing typical monolithic n-tier applications. My quest is to find out how a serverless application actually works and uncover any design patterns that are useful in the process. 

## Prerequisites:
| Tech | Experience |
| --- | --- |
| Basic Electronics | You should know what a breadboard is and be familiar with wiring basic electronic components on one. It’s nice to have a Volt-Ohm-Meter (VOM) as well so you can measure voltages and resitances. |
| Linux/Raspberry Pi | The Rpi is an inexpensive credit card based computer running Linux that has a physical General Purpose Input Output interface or GPIO. This allows us to interface our electronics. You should know how to build a Rpi and expose the GPIO to the breadboard. Additionally, you need to know basic Linux (sudo, ls, pwd, chmod….) |
| Programming | You need to understand the basics of procedural and object based programming in some language. Here we use Python |
| Amazon AWS Account | We are using Amazon Web Services (AWS). Everything we are using is in the free tier. |

# [Return](README.md)