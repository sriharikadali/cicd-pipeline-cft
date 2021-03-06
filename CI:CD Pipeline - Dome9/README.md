# Dome9 - DevSecOps in the CI/CD pipeline

## Setup Dome9 account (www.dome9.com)
1. Make sure you have access to a Dome9 account (www.dome9.com)
1. Generate Dome9 (v2) API keys at https://secure.dome9.com/v2/settings/credentials (and write the key Id and secret)
1. Select a predefined or create a compliance engine bundle and write its ID (visible at the URL)

## Setup the Dome9 Code Pipe line 
1. Create 2 VPCs in your desired account (1 for prod, 1 for test)
1. Create your own S3 bucket in the desired region and enable bucket versions - This will be your artifactory bucket 
1. Review the folder my-app-cft - That Folder should contain your CFT and the parameters JSON files.
   UPDATE the "test-stack-configuration.json" and "prod-stack-configuration.json" files with your VPC IDs. and other environment's parameters
   
1. Review the 2 push___.sh scripts and *modify* your S3 bucket name
1. Upload the pipeline code to S3 by invoking `./push_validation_lambda.sh` (Optional - you can send to the script your aws cli profile as input parameter)
1. Create new stack in the CloudFormation service using `pipeline-cft.json`
1. Fill parameters or take the default values. Note to fill your Dome9 bundle ID 

it'll take about 2 minutes for the pipeline to be created.

## Setting up encrypted parameters (Dome9 credentials)
1. Once the CFN service finishes creating your pipeline, go to the Lambda console and add 2 (encrypted) parameters to:<br/>
 `<stackname>-CFNValidateLambda-*`  and `<stackname>-TestStackValidationLambda-*` <br/>
    1. d9key - your Dome9 v2 API key (that you copied before)
    1. d9secret - your Dome9 v2 secret (that you copied before)

Use the newly created `DevSecOpsPiplineKey` KMS key to encrypt the values

## Money time...
1. Deploy your app code to S3 by running `./push_app.sh`. The code pipeline should catch it automatically and start the pipeline.
