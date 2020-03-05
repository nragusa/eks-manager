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
| us-east-1 | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=eks-manager&templateURL=https://raw.githubusercontent.com/nragusa/eks-manager/master/eks-manager.yml) |
| us-east-2 | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=eks-manager&templateURL=https://raw.githubusercontent.com/nragusa/eks-manager/master/eks-manager.yml) |


