# 🦄 Serverless web application

In this gigantic lab we will build this:

![](<../../.gitbook/assets/image (315).png>)

We will create a simple serverless web application that enables users to request unicorn rides from the Wild Rydes fleet. 🦄

The application will present users with an HTML based user interface for&#x20;

* registering to the service
* requesting rides
* indicating the location where they would like to be picked up.

We will build this lab in steps. The serverless technologies of day 1 we are already familiar with, but there are two new technologies: Amplify and Cognito.&#x20;

Note: AWS Amplify does not currently feature in the AWS Developer certification exam (at least it didn't in November of 2021), but it might show up in the future.&#x20;

## Amplify&#x20;

AWS Amplify allows developers to build and deploy secure, scalable full-stack applications.

## Cognito

Amazon Cognito provides user management and authentication functions to secure the backend API.

## Serverless backend

Amazon DynamoDB provides a persistence layer where data can be stored by the API's Lambda function.

## API Gateway

JavaScript executed in the browser sends and receives data from a public backend API built using Lambda and API Gateway.

