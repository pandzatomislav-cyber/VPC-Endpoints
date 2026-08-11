<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Endpoints

**Project Link:** [View Project](http://nextwork.ai/projects/aws-networks-endpoints)

**Author:** Tomislav Pandza  
**Email:** pandza.tomislav@gmail.com

---

## VPC Endpoints

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_09bcaa8a)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon Virtual Private Cloud (VPC) is a logically isolated section of the AWS Cloud where you can deploy AWS resources such as EC2 instances, RDS databases, and load balancers. It functions like a traditional data center network but leverages AWS’s scalable infrastructure, giving you control over IP address ranges, subnets, routing, and security settings 


### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to create and endpoint and connect it to my subnet. With that, I could test if my policy in the bucket, which stopped all public traffic besides the one from the endpoint, was working. By linking the S3 and EC2 through the VPC Endpoint, I made the resources more secure.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was when policies that block public traffic occur, the only way to fix anything is the CLI and not the web interface.

### This project took me...

This project took me about an hour.

---

## In the first part of my project...

### Step 1 - Architecture set up

In this step, I will:
1. Create a VPC from scratch!
2. Launch an EC2 instance, which you'll connect to using EC2 Instance Connect later.
3. Set up an S3 bucket

### Step 2 - Connect to EC2 instance

In this step, I will connect to my EC2 instance.

### Step 3 - Set up access keys

In this step, I wil give my EC2 instance access to my AWS environment.

### Step 4 - Interact with S3 bucket

In this step, I will try and see if the instance can access our S3 bucket.

---

## Architecture set up

I started my project by launching the EC2.

I also set up an S3 bucket and uploaded some of my files into it.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_4334d777)

---

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I configured my architecture with launching the VPC and EC2, then connect to my EC2 instance, created the Access Key ID and Secret Access Key to be able use the Instance Connect to log in and be able to access S3 bucket and see what it inside.

Access keys are credentials for your applications and other servers to log in into AWS and talk to your AWS services or resources.

The secret access key is like the password that pairs with your access key ID (your username). You need both to access AWS services.
Secret is a key word here - anyone who has it can access your AWS account, so we need to keep this away from anyone else!

### Best practice

Although I'm using access keys in this project, a best practice alternative is to use the IAM roles by assigning a policy for them to be able to access EC2.

---

## Connecting to my S3 bucket

The command I ran was "aws s3 ls". This command is used to display all the buckets I have in S3.

The terminal responded with the list of my buckets. This indicated that the access keys I set up allowed the EC2 to log in ad use the CLI to talk to S3.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_4334d778)

---

## Connecting to my S3 bucket

I also tested the command "aws s3 ls s3://nextwork-vpc-project-tomislav" which returned all the files I had previously uploaded into my bucket.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_4334d779)

---

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command "aws s3 ls". This command created a list of all my buckets inside S3.

The second command I ran was "sudo touch /tmp/test.txt" and "aws s3 cp /tmp/nextwork.txt s3://nextwork-vpc-project-tomislav". The first command created a new file inside the CLI and the second copied the new file from the instance to the bucket.

The third command I ran was "aws s3 ls s3://nextwork-vpc-project-tomislav" which validated that there was indeed a new file inside the S3.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_3e1e79a2)

---

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I will set up a VPC endpoint where my VPC and S3 can communicate directly.

### Step 6 - Bucket policies

In this step, I will block all traffic to S3 bucket besides to one coming from the endpoint. This way we can truly test if all the setup was done correctly and if I can access the bucket without the public internet.

### Step 7 - Update route tables

In this step, I will test the setup of the policy and troubleshoot the connectivity issue.

### Step 8 - Validate endpoint conection

In this step, I will test my VPC endpoint set up and restrict my VPC's access to the AWS environment.

---

## Setting up a Gateway

I set up an S3 Gateway, which is a type of endpoint used specifically for Amazon S3 and DynamoDB (DynamoDB is an AWS database service).
Gateways work by simply adding a route to your VPC route table that directs traffic bound for S3 or DynamoDB to head straight for the Gateway instead of the internet.

### What are endpoints?

An endpoint is a service that allows private connections between your VPC and other AWS services without needing the traffic to go over the internet.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_09bcaa8a)

---

## Bucket policies

A bucket policy is a type of IAM policy designed for setting access permissions to an S3 bucket. Using bucket policies, you get to decide who can access the bucket and what actions they can perform with it.

My bucket policy denies all actions (s3:*) on my S3 bucket and its objects to everyone (Principal: "*"), unless the access is from the VPC endpoint with the ID defined in aws:sourceVpce.
In other words, only traffic coming from my VPC endpoint can get any access to my S3 bucket!

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_7316a13d)

---

## Bucket policies

Right after saving my bucket policy, my S3 bucket page showed 'denied access' warnings. This was because my policy denies all actions unless they come from myVPC endpoint. This means any attempt to access my bucket from other sources, including the AWS Management Console, is blocked!

I also had to update my route table because only traffic that was being routed was the public one. This would not fly with the new policy I installed. By adding the routing through the endpint I should be able to access the S3 through the EC2 now.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_4ec7821f)

---

## Route table updates

To update my route table, I went into the Endpoints and chose the endpoint I needed. In this case it was the NextWork route table which had already been attached to the NextWork VPC when setting up

After updating my public subnet's route table, my terminal could return my bucket name and all the files in my bucket

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_d116818e)

---

## Endpoint policies

Endpoint policies in AWS are resource-based policies attached to VPC endpoints that control access to AWS services, allowing you to specify which actions and resources are permitted.

I updated my endpoint's policy by denying all access to my resources, meaning that the Gateway is closed. I could see the effect of this right away, because my access to the bucket was denied.

![Image](http://nextwork.ai/elated_teal_vibrant_raccoon/uploads/aws-networks-endpoints_3e1e79a3)

---

---
