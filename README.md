# JoomlaSelfService
Solution for automatic set-up and configuration of Joomla production infrastructure on AWS.

## On this Page
- [Deployment](#deployment)
- [Requirements](#requirements)
- [Services](#services)
- [Parameters](#parameters)
- [Installation](#installation)
- [Testing](#testing)

## Deployment
The solution is deployed using 2 CloudFormation templates. 
- ```JoomlaSelfService.json```: Create highly-available, scalable and secure Joomla infrastructure on AWS with a Multi-AZ Amazon RDS database instance for storage. Infrastructure based on stack Centos/Apache/MySQL with 2 Web Server in ELB and Auto scaling mode and RDS MySQL Multi zone.
- ```MaintenancePlan.yaml```: Create Maintenance Plan with backup the web server logs with rotation of 7 days and notification when more than 10 4xx requests are returned by application in a configurable time frame.

## Requirements
- AWS Account
- AWS Management Console (alternatively AWS CLI)
- IAM SSL/TLS Certificate
- Capabilities: CAPABILITY_IAM

## Services
- AWS Cloudformation
- AWS Backup
- AWS CloudWatch
- AWS Fargate
- AWS Lambda
- AWS CodeBuild
- Taurus Automation Framework

## Parameters
### Infrastructure (```JoomlaSelfService.json```)
- Name of an existing EC2 KeyPair to enable SSH access to the instances (line 9)
- Default WebServer EC2 instance type: "m1.small" (line 20)
- The Joomla admin account password (line 27)
- Default Joomla database name: "joomladb" (line 36)
- Default Joomla database admin username: "admin" (line 46)
- Default Joomla database admin password: "password"(line 57)
- Default Database instance type: "db.m1.small"(line 68)
- Default size of the database: "5" (line 76)
- Default create a multi-AZ MySQL Amazon RDS database instance: "true" (line 86)
- Default initial number of WebServer instances: "2" (line 94)
- Default IP address range that can be used to SSH to the EC2 instances: "0.0.0.0/0" (line 101)
- Default AWS Path: "/" (line 110)
- Change name of IAM SSL/TLS Certificate: "name_iam_certificate"(line 177)
- Version download URL of Joomla Stable Full Package, default is version 3.9.14(line 215)

### Maintenance Plan (```MaintenancePlan.yaml```)
- Name of an existing EC2 KeyPair to enable SSH access to the instances (line 5)
- Default IP address range that can be used to SSH to the EC2 instances: "0.0.0.0/0" (line 13)
- Email address to notify if there are any scaling operations (line 17)
- Default time frame in minutes over the number of 4xxs is greater than 10: '1' (line 204)

## Installation
### Infrastructure
1. Login in AWS Management Console
2. Select CloudFormation service
3. Create a new stack uploading the specific template ```JoomlaSelfService.json```
4. Continue and follow the instructions on the screen
4. Execute new created stack

### Maintenance Plan
1. Login in AWS Management Console
2. Select CloudFormation service
3. Create a new stack uploading the specific template ```MaintenancePlan.yaml```
4. Continue and follow the instructions on the screen
4. Execute new created stack

## Testing
### Verifying the solution works fine
1. Go to solution Joomla Administration Website
2. Verify the SSL/TLS Certified charged correctly
3. Insert Administrator user name and password
4. Verify login works fine as Administrator

### Verifying how the solution performs at scale and at load
1. Deploy the [Distributed Load Testing Solution on AWS](https://github.com/awslabs/distributed-load-testing-on-aws/) by GitHub
2. Follow the instructions for configuration
3. Perform stress testing on solution Joomla Website

### Verifying the web fault tolerance of the solution
1. Login in AWS Management Console
2. Select EC2 service
3. Stop one web server instance
4. Verify the solution Joomla Website works fine

### Verifying the database fault tolerance of the solution
1. Login in AWS Management Console
2. Select RDS service
3. Into Database section select your DB instance
4. For Actions, choose Reboot
5. Into Reboot DB Instance page choose Reboot with fail over
6. Wait DB Instance reboots in another Availability Zone, about 60-120 seconds
4. Verify the solution Joomla Website works fine

### Verifying the backup of the web server logs with rotation of 7 days
1. Login in AWS Management Console
2. Select CloudWatch service
3. Into Logs section verify the web server logs backup

### Verifying the notification when more than 10 4xx requests are returned by application in configured time frame (1 minute is default) 
1. Go to solution Joomla Website
2. Add at the end of URL "/MassimoCasale"
3. Click Enter button for 11 times in configured time frame for generating many errors "404 - File Not Found"
4. Login in AWS Management Console
5. Select CloudWatch service
6. Into Overview verify the presence of the notification alarm

Good Job!
