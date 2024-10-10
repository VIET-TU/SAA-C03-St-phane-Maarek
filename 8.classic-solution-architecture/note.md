# Classic Solutions Architecture

# Section Introduction

```
• These solutions architectures are the best part of this course
• Let’s understand how all the technologies we’ve seen work together
• This is a section you need to be 100% comfortable with
• We’ll see the progression of a Solution’s architect mindset through many sample case studies:
    • WhatIsTheTime.Com
    • MyClothes.Com
    • MyWordPress.Com
    • Instantiating applications quickly
    • Beanstalk

```

# Stateless Web App: WhatIsTheTime.com

• WhatIsTheTime.com allows people to know what time it is
• We don’t need a database
• We want to start small and can accept downtime
• We want to fully scale vertically and horizontally, no downtime
• Let’s go through the Solutions Architect journey for this app
• Let’s see how we can proceed!

# Stateless web app: What time is it? Starting simple

![alt text](image.png)

# Stateless web app: What time is it? Scaling vertically

![alt text]({ED2F8589-BCC2-43EA-9679-D624F55F0154}.png)

# Stateless web app: What time is it? Scaling horizontally

![alt text]({93BC80B0-FD25-4CF7-93E4-09C84E529EEE}.png)

# Stateless web app: What time is it? Scaling horizontally

![alt text]({C3A22C7A-7D6E-477A-846F-2AD6C8522EDC}.png)

# Stateless web app: What time is it? Scaling horizontally, adding and removing instances

![alt text]({8C4A7021-31A2-443F-87FA-702F5C9DD30B}.png)

# Stateless web app: What time is it? Scaling horizontally, with a load balancer

![alt text]({6BDCD8AF-E698-42C1-8E34-06D14B3426EF}.png)

# Stateless web app: What time is it? Scaling horizontally, with an auto-scaling group

![alt text]({AFDD1E12-5A93-47E1-AF1D-920E6DA7B995}.png)

# Stateless web app: What time is it? Making our app multi-AZ

![alt text]({0FBB3C1E-B031-4F56-9968-23DC2D62B0D5}.png)

# Minimum 2 AZ => Let’s reserve capacity

![alt text]({664CD552-AABD-4A79-BA09-E82325093AB5}.png)

# In this lecture we’ve discussed…

• Public vs Private IP and EC2 instances
• Elastic IP vs Route 53 vs Load Balancers
• Route 53 TTL, A records and Alias Records
• Maintaining EC2 instances manually vs Auto Scaling Groups
• Multi AZ to survive disasters
• ELB Health Checks
• Security Group Rules
• Reservation of capacity for costing savings when possible

# Stateful Web App: MyClothes.com

• MyClothes.com allows people to buy clothes online.
• There’s a shopping cart
• Our website is having hundreds of users at the same time
• We need to scale, maintain horizontal scalability and keep our web application as stateless as possible
• Users should not lose their shopping cart
• Users should have their details (address, etc) in a database
• Let’s see how we can proceed!

# Stateful Web App: MyClothes.com

![alt text]({1DE97393-CDF5-4473-9EDC-51B973EEFFE1}.png)

# Stateful Web App: MyClothes.com

## Introduce Stickiness (Session Affinity)

![alt text]({4256F672-F2CD-4C95-B74D-6F603AF8732A}.png)

## Introduce User Cookies

![alt text]({84C90CA5-0A90-449E-B83F-809E991DDDEB}.png)

# Introduce Server Session

![alt text]({8F655344-99AD-42EA-98CD-AF2294EB9822}.png)

# Storing User Data in a database

![alt text]({A4BC419E-A148-4E28-BF7C-D9A281970A31}.png)

# Scaling Reads

![alt text]({EA9B6F9F-BEAB-4816-A7B0-C67C5CE80107}.png)

# Scaling Reads (Alternative) – Lazy Loading

![alt text]({5BA38C98-3D1D-4B0D-A2A6-4CB7B9A5A0BD}.png)

# Multi AZ – Survive disasters

![alt text]({0A76820D-E60A-490E-BC5D-40E1352B43A7}.png)

# In this lecture we’ve discussed…

# 3-tier architectures for web applications

• ELB sticky sessions
• Web clients for storing cookies and making our web app stateless
• ElastiCache
• For storing sessions (alternative: DynamoDB)
• For caching data from RDS
• Multi AZ
• RDS
• For storing user data
• Read replicas for scaling reads
• Multi AZ for disaster recovery
• Tight Security with security groups referencing each other

# Stateful Web App: MyWordPress.com

• We are trying to create a fully scalable WordPress website
• We want that website to access and correctly display picture uploads
• Our user data, and the blog content should be stored in a MySQL database.
• Let’s see how we can achieve this!

## RDS layer

![alt text]({47A76F3F-14BE-40A5-8EA1-A933AC1E20A9}.png)

## Scaling with Aurora: Multi AZ & Read Replicas

![alt text](image-1.png)

## Storing images with EBS

![alt text](image-2.png)

## Storing images with EBS

![alt text]({5A255682-2538-4689-9003-27D0712947B2}.png)

## Storing images with EFS

![alt text]({BF415CAD-9A76-4106-9D66-81049F4B5295}.png)

# In this lecture we’ve discussed…

```
• Aurora Database to have easy Multi-AZ and Read -Replicas
• Storing data in EBS (single instance application) • Vs Storing data in EFS (distributed application)
```

# Instantiating Applications quickly

```
 When launching a full stack (EC2, EBS, RDS), it can take time to:
    • Install applications
    • Insert initial (or recovery) data
    • Configure everything
    • Launch the application
• We can take advantage (tận dụng lợi thế) of the cloud to speed that up!
```

# Instantiating Applications quickly

```
• EC2 Instances:
    • Use a Golden AMI: Install your applications, OS dependencies etc.. beforehand ( mọi thứ chuẩn bị trước đó) and launch your EC2 instance from the Golden AMI (và sau đó bạn cài AMI từ nó)
    • Bootstrap (khởi động) using User Data: For dynamic configuration, use User Data scripts
    • Hybrid: mix Golden AMI and User Data (Elastic Beanstalk)
• RDS Databases:
    • Restore from a snapshot: the database will have schemas and data ready!
• EBS Volumes:
    • Restore from a snapshot: the disk will already be formatted and have data!
```

# Typical architecture: Web App 3-tier

![alt text]({342DA47D-FFC9-418C-BECF-C7A1F555E861}.png)

# Developer problems on AWS

```
• Managing infrastructure
• Deploying Code
• Configuring all the databases, load balancers, etc
• Scaling concerns
• Most web apps have the same architecture (ALB + ASG)
• All the developers want is for their code to run!
• Possibly, consistently across different applications and environments
```

# Elastic Beanstalk – Overview

```
• Elastic Beanstalk is a developer centric view of deploying an application on AWS
• It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, …
• Managed service
    • Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration, …
    • Just the application code is the responsibility of the developer
• We still have full control over the configuration
• Beanstalk is free but you pay for the underlying instances
```

# Elastic Beanstalk – Components

![alt text]({666E5D0E-6231-487C-B617-D81B6EA7E09A}.png)

```
• Application: collection of Elastic Beanstalk components (environments versions, configurations, …)
• Application Version: an iteration of your application code (Phiên bản ứng dụng: một phiên bản lặp lại của mã ứng dụng của bạn)
• Environment
• Collection of AWS resources running an application version (only one application version at a time)
• Tiers: Web Server Environment Tier & Worker Environment Tier
• You can create multiple environments (dev, test, prod, …)
```

# Elastic Beanstalk – Supported Platforms

```
• Go
• Java SE
• Java with Tomcat
• .NET Core on Linux
• .NET on Windows Server
• Node.js
• PHP
• Python
• Ruby
• Packer Builder
• Single Container Docker
• Multi-container Docker
• Preconfigured Docker
```

# Web Server Tier vs. Worker Tier

![alt text]({9C40D474-967E-43A7-98FC-A11F82B2431E}.png)

# Elastic Beanstalk Deployment Modes

![alt text]({ED6ACB3F-BBDE-404D-B3DD-6F5A6CFE9BCD}.png)

# Beanstalk Hands On
