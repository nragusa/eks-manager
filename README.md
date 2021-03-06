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

### Create EKS Cluster

There is a tool installed on the EC2 instance called `eksctl` which will be used to create an EKS cluster.Before we run the `eksctl` tool, we need to get the security group ID of our instance:


```
export SG="$(curl -s http://169.254.169.254/latest/meta-data/network/interfaces/macs/$(curl -s http://169.254.169.254/latest/meta-data/mac)/security-group-ids)"
```

Now we can use the output from above when we create our EKS cluster:

```
eksctl create cluster \
    --name bdc \
    --region us-east-1 \
    --tags "Environment=Dev,Project=BigDataCluster,Name=bdc-node" \
    --version 1.15 \
    --nodes 3 \
    --node-type m5.large \
    --nodegroup-name workers \
    --node-volume-size 100 \
    --vpc-private-subnets=subnet-XXXXXX,subnet-XXXXXX,subnet-XXXXXX \
    --node-private-networking \
    --ssh-access \
    --ssh-public-key XXXXX \
    --node-security-groups $SG
```

Be sure to replace `vpc-private-subnets` and `ssh-public-key` with values from your environment!

This process can take 15 or more minutes to complete. You can check the progress of the command by logging in to the CloudFormation console and checking on the stack deployment progress.

### Launch a Big Data Cluster

Once the cluster is up and running, you can now deploy Microsoft SQL Server 2019 Big Data Cluster using the pre-installed `azdata` tool. An example command:

```
azdata bdc create --accept-eula=yes
```
![azdata wizard](images/bdc2.png)

As the Big Data cluster is deployed, it will output the IP for the cluster endpoint. You'll need this for connecting to the cluster, so make sure you leave this window open!

### Connect to the BDC

Once the BDC deployment completes, you can connect to the cluster with `azdata`. Using the endpoint that was echoed during the deployment, run the following:

```
azdata login --endpoint https://<ip or domain name>:30080
```

and specify the username / password combination you specified when you created the cluster.