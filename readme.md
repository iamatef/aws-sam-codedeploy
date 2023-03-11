# AWS SAM + CodeDeploy
This is a demo of how to use AWS SAM to deploy a python app to AWS CodeDeploy

## 1  download a sample app  
````
sam init --runtime python3.9
````

Choose aws quick start template hello world, this should clone a repo and then you have an app in directory sam-app so 

````
cd sam-app
````

## 2  Modify the template.yaml file to the sam-app directory to add canary deployments
````
      AutoPublishAlias: live
      DeploymentPreference:
       Type: Canary10Percent10Minutes 
````

## 2 build the app
````
cd sam-app
sam build
````

## 3 deploy the app
````
sam deploy --guided
````

This will ask you for a stack name, a bucket name, and a region.  It will also ask you if you want to deploy the app.  Say yes.

this will deploy the stack and create the lambda function that shows hello world when tested, it also adds a live alias for canary deployment and traffic shifting.

API Gateway is created with an endpoint that forward traffic to the lambda function. 
sam-app-HelloWorldFunction-wCUuuA8vB1df:live

Notice the alias which is weighted to 100% of traffic.


## 4  modify the app to test canary deployment

1. Now change the message from hello world to hello world 2 in the app.py file
2. run sam build again
3. run sam deploy again 

This will deply the new version of the app and change the alias to 10% of traffic to the new version and 90% to the old version. Check CodeDeply to see the deployment in progress.  After 10 minutes the alias will be changed to 100% of traffic to the new version and 0% to the old version.  Check the API Gateway endpoint to see the new message.
