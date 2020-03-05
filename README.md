## Overview

The following CloudFormation template has been built to help simplify the deployment of Microsoft SQL Server 2019 Big Data Cluster on AWS. Once deployed, this template will create:
* An EC2 instance with the following:
  * The `azdata` tool installed for managing the Big Data Cluster
  * `eksctl` to create an EKS cluster on AWS
  * An instance profile attached to the instance so you can use the tools above

 
### Deploy Stack

The first step is to deploy a stack using the provided CloudFormation template. Click the appropriate button below to launch in your prefered region:

| Region    | Stack                                                                                                                                                                                                                                                                                               |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| us-east-1 | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=eks-manager&templateURL=https://ragusan-cloudformation.s3.amazonaws.com/eks-manager.yml) |
| us-east-2 | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=eks-manager&templateURL=https://ragusan-cloudformation.s3.amazonaws.com/eks-manager.yml) |


There are 2 parameters that must be filled in that are specific to your environment:

![Parameters](images/parameters.png)

The remaining parameters have suitable defaults.

### Connect to the Instance

To connect to the EC2 instance, use [Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html).

![Session manager](images/ssm.gif)

In your AWS account:
1) Go to the EC2 console and find the instance named `eks-manager` (or the whatever instance name you gave it) 
2) Click the __Connect__ button at the top
3) Select the __Session Manager__ radio button
4) Click the blue __Connect__ button