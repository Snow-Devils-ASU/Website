# Website
The Snow Devils @ ASU website intended to be hosted on AWS.

## Overview
So you want to modify the Snow Devils @ ASU's website? You've come to the right place. All of the website and its configuration is stored 
in this repository. This website is deployed to an Amazon Web Services (AWS) account to make it broadly available on the internet.

A website is simply a collection of files that are returned to a user's computer when the user "browses" to that site. The user's
web browser will render those files on the screen visually for convenience. The entire Snow Devils' website is stored in the `docroot/` folder.
This is a collection of files that define what the website says and how it looks. 

However, editing any of the files in `docroot/` will only change the website if viewed locally. To share the website with the rest of the world, we need to deploy it and make it publicly available.
AWS makes that easy for us. AWS will publicly _host_ this website on the internet for us for very low fees. After making any changes you want to the website in `docroot/`, you will need to upload it 
to AWS.

### AWS System Overview
Amazon Web Services is a collection of tools (services) business can use to solve common technical problems.
_The specific services we are using to deploy this website are called: [S3](https://s3.console.aws.amazon.com/s3/buckets), [CloudFront](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions), [Route53](https://us-east-1.console.aws.amazon.com/route53/v2/hostedzones?region=us-east-1#), [Certificate Manager](https://us-east-1.console.aws.amazon.com/acm/home?region=us-east-1#/certificates/list), and [CloudFormation](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks)._

* S3: Is a storage service, like Google Drive, that lets us drop files in and then access them. The website will be stored here in a "bucket" (a drive).
* CloudFront: Distributes the website from S3 globally.
* Route53: Handles domain registration and DNS. Here is where you would manage renewing your domain name (snowdevilsasu.com) or buying another one. Administrators please see [Domain Readme](Domain%20Readme.md).
* Certificate Manager: Modern websites are all secured using TLS/SSL certificates. This handles making a certificate for the website.
* CloudFormation: See below.

The collection of these services taken together, the way they are configured, and the way they interact with each other for our particular
project is called a "stack". CloudFormation is an AWS service to help you manage your stacks easier. You give CloudFormation a template file which specifies every AWS resource you want to use and how you want it configured,
 and CloudFormation will set up all of those services on your behalf. The files in the `stack/` directory are such template files. To modify any part of this website stack, edit those template files.

There are 2 parts to deploying the website on AWS:
1. Setting up the stack which includes setting up things such as the S3 bucket, DNS, Certificates, CloudFront distributions, etc. All the technical things needed to make a website accessible to users.
2. Uploading the website files (the `docroot/` folder) to the AWS S3 bucket

Step 1 creates an S3 bucket (think Google Drive folder) and then in step 2 you upload your website files to that bucket. It is expected that step 1 would be performed ideally once to initially create the bucket. Then every time anyone would like to 
edit the website content, they would make any appropriate changes to the local `docroot/` files and just upload the files to the bucket again (step 2).

## Development Prerequisites
Install the [AWS CLI](https://aws.amazon.com/cli/) and the [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html).

Authenticate yourself by running ``aws configure --profile snowdevil`` and fill out the prompts with the Keys provided to you by the account administrator.

*If you are the Account Administrator, follow [these steps](Permissions%20Readme.md) to onboard a new developer*

## Deploy the Stack
The AWS stack can be deployed in 2 different environments: Dev (development) and Prod (production). Development allows us to test changes before deploying them in Prod for the whole world to see.

Deploying these respective stacks really should only need to be done once. However running the subsequent commands multiple times will not be harmful.

To deploy the dev stack run:
``sam deploy --profile snowdevil --template stack/RootStack.yaml --resolve-s3 --config-env dev``

To deploy the prod stack run:
``sam deploy --profile snowdevil --template stack/RootStack.yaml --resolve-s3 --config-env prod``

If you make any changes to the templates in `stack/`, then run the above commands again to update the AWS stacks.

## Upload the Website Files
Now that the dev and prod stacks are deployed, you must upload the website files to the appropriate S3 bucket to have them distributed publicly.
When you run the above `deploy` commands, they return the `S3WebsiteBucket` name where the website files should be stored. The dev and prod stacks have their own buckets.
1. Grab the name of the `S3WebsiteBucket` from the appropriate stack that you would like to upload to above
2. Run ``aws s3 sync docroot/ s3://WEBSITEBUCKET --delete --profile snowdevil`` to upload the content of the `docroot/` folder to the S3 bucket. Replace `WEBSITEBUCKET` with the name of the bucket.
3. Check your changes by navigating to the appropriate website. Note: The production website has caching enabled that will cache site content for about a day. It may take a day for you to see site changes on the prod website after they've been uploaded to the bucket. The dev website does not cache content.

For example, if I would like to upload to the dev website, I might execute ``aws s3 sync docroot/ s3://beta.snowdevilsasu.com-randomchars --delete --profile snowdevil`` where `beta.snowdevilsasu.com-randomchars` was the output for `S3WebsiteBucket` from the deployment command above.
Then I can browse to [beta.snowdevilsasu.com](https://beta.snowdevilsasu.com) to view my site.

## Delete a Stack
1. Remove everything from the S3 bucket: ``aws s3 rm s3://BUCKET_NAME --recursive --profile snowdevil``
2. Run the following command for whichever stack you are deleting:
    a. Dev: ``sam delete --region us-east-1 --profile snowdevil --stack-name Website-Dev``
    b. Prod: ``sam delete --region us-east-1 --profile snowdevil --stack-name Website-Prod``

If you do not clean out the bucket, then the stack will fail to delete. Remove everything and then try to delete the stack again.

# Editing the Website Files
To keep this website simple, we will only make this website comprised of 1 file: the `index.html` file. Any edits you would like to make to the website and its content should be made inside this file. You will also find helper files
such as those in the `assets/` folder - these files are consumed by `index.html` and help it style itself. Please don't touch the helper files unless you're confident in
what you're doing.

When editing the `index.html` locally, you can open it in your browser locally to preview any changes before having to deploy it to AWS. Normally double-clicking on the file will open it in your browser. Make a change, then reload the page and see it updated.

When you're ready to try out your changes on the actual site, follow the steps above in [Upload the Website Files](#Upload%20the%20Website%20Files) to upload your files to the dev website first. Verify everything looks okay on beta.snowdevilsasu.com and then upload to the prod website.