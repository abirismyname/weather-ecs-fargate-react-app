# weather-ecs-fargate-react-app

## Bootstrap 

<prerequisites such as AWS CLI>
<placeholder instruction to grab AWS credentials from event engine>

Populate the environment variables on your command line
```
GITHUB_ORG=<enter the GitHub Org Name>
GITHUB_REPO=<enter the GitHub repository name>
ROLE_NAME=<enter the IAM role for AWS configure credentials action>
ECR_REPO=<enter the ECR repository name>
AWS_REGION=<enter the target AWS region, use us-east-1>
AVAILABILITY_ZONES=us-east-1a,us-east-1b,us-east-1c
ECS_CLUSTER_NAME=<enter the ECS cluster name>
```

Bootstrap the AWS account using the following command
```
aws cloudformation deploy --template-file ./templates/bootstrap.yaml --stack-name bootstrap --parameter-overrides GitHubOrganization=$GITHUB_ORG RepositoryName=$GITHUB_REPO RoleName=$ROLE_NAME ECRRepositoryName=$ECR_REPO AvailabilityZones=$AVAILABILITY_ZONES ECSClusterName=$ECS_CLUSTER_NAME --region $AWS_REGION --capabilities CAPABILITY_NAMED_IAM
```

Get the stacks outputs 
```
aws cloudformation describe-stacks --stack-name bootstrap --region us-east-1 | jq ".Stacks[0].Outputs[]"
```

Create a new environment based of the branch name. Configure vars and secrets:

Secrets: 
* `ROLE_TO_ASSUME` = the output value of `RoleGithubActionsARN` from CloudFormation output

Vars: 
* `AWS_REGION` = the output value of 'AWSRegion' from CloudFormation output
* `ECR_REPO_NAME` =  the output value of `ECRRepositoryName` from CloudFormation output
* `VPC_STACK_NAME` =  the output value of `VPCStackName` from CloudFormation output
* `ECS_CLUSTER_NANE` =  name of ECS cluster to be created
* `APP_NAME` = name of the application shown in ECS