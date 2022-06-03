# Domain Management Instructions
This document is intended for the Account Administrator. This document explains how to manage the domain in AWS.

The Snow Devils domain is managed in [Amazon Route53](https://us-east-1.console.aws.amazon.com/route53/v2/hostedzones?region=us-east-1#).

## View Registration and Renew Domain
1. Navigate to the [Route53](https://us-east-1.console.aws.amazon.com/route53/v2/hostedzones?region=us-east-1#) management console.
2. Click "Registered Domains" on the left.
3. View and manage domain registration. You can edit settings here such as Transfer Lock and Auto Renew. Note that Transfer Lock should be on.

## Manage the Hosted Zone
The Hosted Zone for the domain is where any DNS records will be placed to inform the world how to find and navigate our domain.
1. Navigate to the Route53 console
2. Click Hosted Zones on the left
3. _There should be 1 and only 1_ hosted zone listed per domain registered. It should be named the same as the domain registered.
4. The nameservers inside this hosted zone should match what nameservers are listed in the Domain Registration page for the domain. If not, please remediate: [https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/MigratingDNS.html](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/MigratingDNS.html)

**If either the Domain Name or the Hosted Zone change, you MUST update the configuration file [samconfig.toml](stack/samconfig.toml) with the new HostedZoneID and HostedZoneDomain.** The hosted zone id is found in the hosted zone page, and the hosted zone domain is the domain name that you have registered. 