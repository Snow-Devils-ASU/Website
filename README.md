# Website
The Snow Devils @ ASU website intended to be hosted on AWS.

## Prerequisite 
Install the AWS CLI and the AWS SAM CLI.

## Deployment Instructions
You must first be authenticated with AWS using ``aws configure --profile snowdevil``


``sam deploy --template stack/RootStack.yaml --stack-name Website-Dev --config-env dev --resolve-s3``


Delete a stack
``sam delete --region us-east-1 --profile snowdevil --stack-name Website-Dev``


## Uploading website
``aws s3 sync docroot/ s3://snowdevilsasu.com-dev --delete --profile snowdevil``