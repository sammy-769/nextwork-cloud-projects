<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Endpoints

**Project Link:** [View Project](https://nextwork.ai/projects/749fab76-0a0b-5822-9b31-c7863050c9bd)

**Author:** Sammy Shah Saleh  
**Email:** sammysaleh769@gmail.com

---

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_09bcaa8a)

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) allows me to create an isolated virtual network in AWS where I can launch and manage resources such as EC2 instances.

### How I used Amazon VPC in this project

In this project, I created a VPC with a public subnet, an Internet Gateway, a route table and an EC2 instance. I then configured an S3 Gateway VPC endpoint and tested access to my S3 bucket using bucket and endpoint policies.

### One thing I didn't expect in this project was...

One thing I didn't expect was how easy it was to set up a VPC endpoint. I thought the process would be difficult and complicated, but I found it more straightforward than I had expected.

### This project took me...

This project took me more than two hours to complete, including the documentation.

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I created a VPC from scratch, launched an EC2 instance that I could connect to using EC2 Instance Connect, and created an Amazon S3 bucket for testing access through a VPC endpoint.

### Step 2 - Connect to EC2 instance

In this step, I connected to my EC2 instance using EC2 Instance Connect so that I could use the AWS CLI to interact with Amazon S3.

### Step 3 - Set up access keys

In this step, I configured AWS access keys on my EC2 instance so that the AWS CLI could authenticate with AWS services and interact with my AWS environment.

### Step 4 - Interact with S3 bucket

In this step, I used the AWS CLI to access my Amazon S3 bucket, vpc-endpoint-123, and list the objects stored in it. I also uploaded a text file from my EC2 instance to the bucket to confirm that the upload worked.

## Architecture set up

I created a VPC with one public subnet, an Internet Gateway and a route table. I also launched an EC2 instance inside the VPC, which I used to test access to Amazon S3 through a VPC endpoint.

I created an Amazon S3 bucket named vpc-endpoint-123 and uploaded two image files to it. I later used this bucket to test access through the VPC endpoint and verify that the endpoint configuration worked.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d777)

## Access keys

### Credentials

The four settings I configured using the `aws configure` command were the AWS Access Key ID, AWS Secret Access Key, default Region name (`eu-north-1`), and default output format. These settings allow the AWS CLI to authenticate my requests and determine the default Region and format for displaying command output.

AWS access keys consist of an access key ID and a secret access key and are used to authenticate programmatic requests to AWS. The AWS CLI uses these credentials to make requests on my behalf, while the permissions associated with my AWS identity determine which resources I can access. I must keep my secret access key private and never share it publicly or commit it to a GitHub repository.

A secret access key is a private credential paired with an AWS access key ID. It is used to sign and authenticate requests to AWS services. I must keep it secure and never share it publicly because anyone who obtains it may be able to access AWS resources, depending on the permissions associated with the credentials.

### Best practice

The best-practice alternative to using access keys is to attach an IAM role to my EC2 instance. This allows the instance to obtain temporary security credentials automatically without storing long-term access keys on the instance. However, I will continue using access keys in this project rather than configuring an IAM role.

## Connecting to my S3 bucket

The command I ran was `aws s3 ls`. This command lists the Amazon S3 buckets that my configured AWS credentials have permission to list.

I ran `aws s3 ls` to list the Amazon S3 buckets that my configured AWS credentials had permission to list. After configuring my credentials, I ran the command again and the terminal displayed my accessible buckets, confirming that the AWS CLI could successfully make requests to Amazon S3.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d778)

## Connecting to my S3 bucket

The command I ran was `aws s3 ls s3://vpc-endpoint-123`. It listed the two image files stored in my S3 bucket, confirming that my EC2 instance could access the bucket and list its objects using the AWS CLI.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d779)

## Uploading objects to S3

The first command I ran was `sudo touch Hello_world.txt`, which created an empty text file named `Hello_world.txt` on my EC2 instance.

The second command I ran was `aws s3 cp Hello_world.txt s3://vpc-endpoint-123`, which uploaded the text file from my EC2 instance to my S3 bucket.

The third command I ran was `aws s3 ls s3://vpc-endpoint-123`. The terminal listed `Hello_world.txt` alongside the two image files, confirming that the file had been uploaded successfully and was accessible in my S3 bucket.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a2)

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I created an S3 Gateway VPC endpoint to allow traffic from my VPC to reach Amazon S3 through the endpoint rather than through a public internet route.

### Step 6 - Bucket policies

In this step, I configured an S3 bucket policy to deny requests that did not come through my specified VPC endpoint. This allowed me to test whether access to my bucket was being restricted to traffic using the endpoint.

### Step 7 - Update route tables

In this step, I associated the S3 Gateway VPC endpoint with the route table used by my public subnet so that traffic destined for Amazon S3 could be routed through the endpoint. I then tested access from my EC2 instance and troubleshot the configuration.

### Step 8 - Validate endpoint conection

In this step, I tested access to my S3 bucket again from my EC2 instance. I also changed the endpoint policy to test how it affected access to Amazon S3.

## Setting up a Gateway

An S3 Gateway VPC endpoint provides a route from my VPC to Amazon S3 without sending S3 traffic through the public internet. When I associate the endpoint with a route table, AWS adds a route for Amazon S3 traffic through the endpoint. This allows resources in the associated subnets to access S3 through the endpoint without needing an internet gateway or NAT gateway for that traffic.

### What are endpoints?

In AWS, a VPC endpoint provides a way for resources in a VPC to connect to supported AWS services without requiring a public internet route for that connection. The type of endpoint determines how the connection works. In this project, I used an S3 Gateway VPC endpoint.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_09bcaa8a)

## Bucket policies

An S3 bucket policy is a JSON document that defines which requests are allowed or denied access to an Amazon S3 bucket and its objects.

My bucket policy explicitly denies all S3 actions on the `vpc-endpoint-123` bucket and its objects when a request does not come through my specified VPC endpoint, `vpce-05df52053f4f0072f`. Requests that use this endpoint are not denied by this particular policy statement, but they must still satisfy any other applicable permissions and policies. I used this policy to test whether my EC2 instance was accessing S3 through the VPC endpoint.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_7316a13d)

## Bucket policies

I received access-denied warnings when trying to view or manage my S3 bucket through the AWS Management Console. This happened because I updated the bucket policy to deny all S3 actions when requests did not come through my specified VPC endpoint. 

The policy therefore restricted access based on the request's network path. Requests through the specified endpoint were not denied by this policy statement, but they still needed to meet any other applicable permission requirements.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4ec7821f)

## Route table updates

I realised that my route table did not yet have a route for Amazon S3 traffic through the VPC endpoint. I associated the S3 Gateway VPC endpoint with the route table used by my public subnet, allowing traffic destined for S3 to use the endpoint instead of following the public internet route.

After updating the route table, I ran the S3 listing command from my EC2 instance again. The terminal successfully displayed the objects stored in my S3 bucket, including the two image files and `Hello_world.txt`. This confirmed that my EC2 instance could access the bucket through the VPC endpoint while the bucket policy was restricting requests that did not use it.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_d116818e)

## Endpoint policies

An endpoint policy is a policy attached to a VPC endpoint that controls which AWS resources and actions can be accessed through that endpoint. It works alongside IAM policies and resource policies, such as S3 bucket policies, rather than replacing them.

I changed the endpoint policy statement's "Effect" value from "Allow" to "Deny". This denied requests matching that statement, preventing my EC2 instance from interacting with Amazon S3 through the endpoint. This demonstrated that even when the route table is configured correctly, an endpoint policy can restrict access to AWS services.

![Image](https://nextwork.ai/sammy769/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a3)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/749fab76-0a0b-5822-9b31-c7863050c9bd)*
