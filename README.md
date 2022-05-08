# Website
The Snow Devils @ ASU website intended to be hosted on AWS.

## Deployment Instructions
You must first be authenticated with AWS using ``aws configure --profile snowdevil``


``sam deploy --template RootStack.yaml --stack-name Website-Dev --config-env dev --resolve-s3``


Delete a stack
``sam delete --region us-east-1 --profile snowdevil --stack-name Website-Dev``


## Uploading website
``aws s3 sync docroot/ s3://snowdevilsasu.com-dev --profile snowdevil``