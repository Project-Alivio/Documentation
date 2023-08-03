# Overview

Alivio primarily uses the following AWS Services: 

- ECR: Elastic Container Registry for storing Docker Images. 
- EC2: An elastic load balancer to handle changing user demand. 
- RDS: A relational database service.
- WAF: A web application firewall for security. 

This document covers the following: 

- Deploying the application to the ECR (Elastic Container Registry) and loading it into the ELB (Elastic Load Balancer) 
- The web application firewall (WAF) 

## Deploying to ECR, pushing to ELB. 

To redeploy the application on AWS, follow the steps in the section below: 

- The main branch of the application has a file named deploy-image.sh running this script will push the docker image to ECR. 
- If the script stops working or isn’t available for whatever reason, the commands for the script can be found at the AWS Management Console. 
    - Google > AWS Management Console > Services > ECR > Repositories > alivio-testing -> View Push Commands 

**Now do the following**: 

1. Run the script: `$ bash deploy-image.sh`
2. Go to: AWS Management Console > Elastic Beanstalk > Environments > environment_name (e.g Aliviotest1-env)  > Click Upload and Deploy > Select Dockerrun.aws.json 
3. Done! 

## WAF (End to End Encryption) 

This section details how to add a new ACL (Access Control List) and link it to an existing application. It requires the following steps already be completed: 

- A docker image has been created and pushed to ECR, and that it is also on an ELB.
- There is a ALB (Application Load Balancer) that is linked to an EC2 Instance. 

**To add more rules to the ACL**:

Google > AWS Management Console > Services > WAF and Shield > Web ACL’s > Region=US-east-2 (ohio) 

Currently the only ACL is alivioTestingACL, in the future if we ever need to make a new ACL (development vs live use), follow these steps: 

Go to the ACL: google > AWS Management Console > Services > WAF and Shield > Web ACL’s, click: create web ACL, use the following fields: 

- Resource Type: Regional resources (we’re using an ALB) 
- Region: US-East (Ohio) 
- Associated AWS resources = `<EC2-instance-name>` 

    (i) The console states this step is optional in the start-up process, it is NOT optional, here you must link your ACL to the EC2 instance. Without this step the ACL will not stand in-front of any requests for the ALB. 

    (ii) If (i) doesn’t work, it should also be possible to go to Services > EC2 > instance_name > security groups and then link the ACL there (although I didn’t have success with this method at the time of writing). 

Once the ACL has been generated, add any rules you need, the testing-build uses the following. 

- A regional rule (only allow access from Guatemala, US) 
- Rule for SQL injections 
- General AWS safety rules 

Done!