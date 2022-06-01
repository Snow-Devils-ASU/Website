# Website
The Snow Devils @ ASU website intended to be hosted on AWS.

## Prerequisite 
Install the AWS CLI and the AWS SAM CLI.

## Deployment Instructions
You must first be authenticated with AWS using ``aws configure --profile snowdevil``


``sam deploy --profile snowdevil --template stack/RootStack.yaml --resolve-s3 --config-env dev``


Delete a stack
``sam delete --profile snowdevil --stack-name Website-Dev``


## Uploading website
``aws s3 sync docroot/ s3://snowdevilsasu.com-dev --delete --profile snowdevil``