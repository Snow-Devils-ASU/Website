# Permissions Instructions
This document is intended for the Administrator of the AWS account. This document explains how to set up proper permissions and maintain accounts for developers working on the website.

## Set up Dev Group
Fist create a Route53 policy
1. Go to [IAM Portal](https://us-east-1.console.aws.amazon.com/iamv2/home)
2. Click on Policies on left
3. Create a new policy, click JSON on the top
4. Enter the following into the text box:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "route53:*",
        "route53domains:CheckDomainTransferability",
        "route53domains:CheckDomainAvailability",
        "route53domains:ListDomains",
        "route53domains:ListTagsForDomain",
        "route53domains:ListOperations",
        "route53domains:ViewBilling",
        "route53domains:GetDomainSuggestions",
        "route53domains:GetContactReachabilityStatus",
        "route53domains:GetDomainDetail",
        "route53domains:ListPrices",
        "route53domains:GetOperationDetail",
        "cloudfront:ListDistributions",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticbeanstalk:DescribeEnvironments",
        "s3:ListBucket",
        "s3:GetBucketLocation",
        "s3:GetBucketWebsite",
        "ec2:DescribeVpcs",
        "ec2:DescribeVpcEndpoints",
        "ec2:DescribeRegions",
        "sns:ListTopics",
        "sns:ListSubscriptionsByTopic",
        "cloudwatch:DescribeAlarms",
        "cloudwatch:GetMetricStatistics"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "apigateway:GET",
      "Resource": "arn:aws:apigateway:*::/domainnames"
    }
  ]
}
```
5. Click Next, click Next, then enter LimitedRoute53Access as the name and save.

THEN,
Create a group called SnowDevilDeveloper for new users to use to develop on the website.
1. Go to [IAM Portal](https://us-east-1.console.aws.amazon.com/iamv2/home#/groups) signed in as Admin.
2. Click `User Groups` on the left
3. Create a new user group called SnowDevilDeveloper
4. Attach the following permission policies to the group: AmazonS3FullAccess, CloudFrontFullAccess, AWSCertificateManagerFullAccess, AWSCloudFormationFullAccess, LimitedRoute53Access
5. Click save

Or you can do the above by just deploying using CF by one time execute: ``sam deploy --profile snowdevil --template stack/DevPermissions.yaml --resolve-s3 --stack-name Dev-Permissions --region us-east-1 --capabilities CAPABILITY_NAMED_IAM``

## Onboard New Developer
To grant someone access to work on the website (including yourself, admin), follow these steps.
1. Go to [IAM Portal](https://us-east-1.console.aws.amazon.com/iamv2/home#/groups)
2. Click Users on the left
3. Click Add User
4. Give them a user name
5. Select "Programmatic Access". If you'd like, you can also select "Password". By selecting "Password" you will grant them access to the AWS management console as well (but no extra permissions)
6. Click "Next: Permissions"
7. Click Add User to Group, and select the SnowDevilDeveloper group (created above).
8. Click Next then Next then "Create User"
9. Now you will be presented with a Success page. On this page is important information.
10. On the line for the new user created, there will be an Access Key ID and a Secret Access Key. Copy and send both of those to your new developer.
11. It is very important you do the above step because after you click "Close", you will not be able to see the Secret Access Key again. If you missed copying it, delete the user and start over.
12. Now direct your new developer to run ``aws configure --profile snowdevil`` in their shell and fill out the prompts with the Keys you just provided them.

## Delete old Developer
1. Go to [IAM Portal](https://us-east-1.console.aws.amazon.com/iamv2/home#/groups)
2. Click Users on the left
3. Select user and delete them