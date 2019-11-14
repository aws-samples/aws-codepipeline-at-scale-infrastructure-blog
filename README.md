## Using AWS CodePipeline and open source tools for at-scale infrastructure deployment

Welcome to our blog post! Join us to build a serverless infrastructure deployment pipeline using AWS developer tools in conjunction with popular open source tools such as: CFN-Nag (https://github.com/stelligent/cfn_nag), CFN-Python-Lint (https://github.com/aws-cloudformation/cfn-python-lint), and Stacker (https://github.com/cloudtools/stacker). The pipeline will run automated validation checks against AWS CloudFormation templates and deploy the corresponding CloudFormation stacks.

The included sample files provide instructions on how to deploy:

* An AWS CodePipeline pipeline to provision infrastructure as code using open source tools
* An AWS Identity and Access Management (IAM) role required by Stacker to deploy to these AWS resources: Amazon Elastic Compute Cloud (EC2), Simple Storage Service (S3) and IAM.



High Level Architecture 

![pipeline_diagram.pdf](https://github.com/aws-samples/aws-codepipeline-at-scale-infrastructure-blog/tree/master/blog)

Pre-requisites:

* Git client (https://git-scm.com/downloads) 
* AWS Account (https://aws.amazon.com/account/)
* AWS CLI (https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) , latest version
* Python 3.6+ (https://www.python.org/downloads/)
* CFN-Nag (https://github.com/stelligent/cfn_nag)
* CFN-Python-Lint (https://github.com/aws-cloudformation/cfn-python-lint)
* Stacker (https://pypi.org/project/stacker/)

Details are provided in the blog post on how to install CFN-Nag, CFN-Python-Lint and Stacker.


Instructions

Open the CloudFormation console in an account you have access to:

1. Get the source code from the aws-samples repository
2. Create a local Git repository for the pipeline artifacts
3. Edit the Stacker-Profile File
4. Edit the Stacker-Configuration File
5. Provision the PIpeline with three stacks:
    1. Pipeline Stack 1: Creates a private S3 bucket
    2. Pipeline Stack 2: Creates an EC2 IAM role that allows putting objects in the S3 bucket that was created as part of Stack 1.
    3. Pipeline Stack 3:  creates an EC2 instance and EC2 profile that assumes the IAM role created in stack 2 and uploads a simple file to the S3 bucket created in stack #1. Stacks 1 and 2 are a dependency for Stack 3. 
6. Trigger the AWS CodePipeline
7. Test a Pipeline Failure Scenario
8. Cleanup

File Structure

Our project filestructure will be organized as stated below:


* codepipeline/: CloudFormation (CFN) templates to deploy the solution (CodePipeline + Stacker IAM role) stacker/buildspec.yaml: AWS CodeBuild buildspec file that installs and invokes Stacker for CFN provisioning 
* stacker/stacker-config.yaml: Stacker configuration file containing stack descriptions and input parameters 
* stacker/stacker-profiles: the AWS profiles Stacker will use to assume roles during deployment 
* templates/*: the sample templates that will be validated and deployed

Testing Failure Scenarios of the Pipeline

We will make a change to one of the templates to cause the pipeline template verification stage to fail and understand how to investigate and to underscore the importance of the open source tool, CFN-Lint integrated with AWS Developer Tools, in our project. 

Costs

Please be aware that the CodePipeline pipeline will deploy resources in your account (S3 bucket, EC2 instance) that will incur minor costs.




## License

This library is licensed under the MIT-0 License. See the LICENSE file.

