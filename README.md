# Redirect HTTP/HTTPS Traffic from WWW to the Domain Root

Easily redirect HTTP and HTTPS traffic at WWW.[domain.tld] to the root of the domain [domain.tld].

## Using CloudFront, S3 and Route53

This repository provides an AWS CloudFormation Template to construct a CloudFront SSL/HTTPS endpoint, an S3 bucket for redirection and correct Route53 DNS entries.

## Automated Usage

You can download the automated script for this repo and run it yourself for your deployment:

[COMING SOON]

## Prerequisites for the Manual Usage

You will need the following, which are all automated but good to understand what assets you need to stand up in the stack.

### Determine the Hosted Zone ID

Determine the zone ID using the AWS CLI. In this example I'll use my named profile `example` and look for `example.com`

#### Using the AWS CLI

Please note you'll need `jq` for this operation to work.  If you are on MacOS, for example, you can add it with brew: `brew install jq`

```sh
#!/bin/zsh

# Prompt for user input
echo "Enter AWS CLI Profile Name:"
read profile

echo "Enter Domain:"
read domain

# Fetch HostedZoneID
aws route53 list-hosted-zones-by-name --profile=$profile |
jq --arg name $domain \
-r '.HostedZones | .[] | select(.Name=="\($name)") | .Id'
```

#### Example output:

```
/hostedzone/Z1UVA2VESUQ1UN
```

## Manual Usage of the Stack Template
```
aws cloudformation create-stack --stack-name www-redirect --template-body file://redirect-www-to-root.yml \
--parameters \
ParameterKey=DomainName,ParameterValue=example.com \
ParameterKey=HostedZoneID,ParameterValue=Z1UVA2VESUQ1UN \
--region=us-east-1 \
--profile=example
```