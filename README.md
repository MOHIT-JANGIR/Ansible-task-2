# Web Hosting on AWS With Ansible Using Dynamic Inventory Concept
![image](https://user-images.githubusercontent.com/61896468/97162974-f5741400-17a5-11eb-9026-8573ed130221.png)


# Task 2 -
# Statement : Deploy Web Server on AWS through ANSIBLE!

## Provision EC2 instance through ansible.

## Retrieve the IP Address of instance using dynamic inventory concept.

## Configure the web server through ansible!

## Create role for webserver to customize the Instance and deploy the webpage to root directory. 

# *Lets something know about Ansible and AWS -*
##  *Ansible is tool which is used for Configuration Management (CM). In Ansible we only tell to "what to do". We don't need to tell "How to do" because Ansible already knows that "How to do this task" on each type of Operating System .*

## `AWS is a cloud which is provided by Amazon. Amazon Elastic Compute Cloud (Amazon EC2 ) is a part of Amazon.com's cloud-computing platform, Amazon Web Services, that allows users to rent virtual computers on which to run their own computer applications. AWS EC2 service provisions resources like RAM , HardDisk . CPU etc.`

# To communicate with AWS we need -

```
> "boto" API

> Access key and Private key (To login in aws)
```

# Step - 1 Install "boto" & "boto3" libraries -
![image](https://user-images.githubusercontent.com/61896468/97164653-792f0000-17a8-11eb-917d-c0a0de676486.png) 
## I already have access key and private key to login in AWS.

# Step - 2 Create Ansible Configuration File -
![image](https://user-images.githubusercontent.com/61896468/97164607-661c3000-17a8-11eb-8426-8bd1f3ec8e13.png) 
## I have already "hello.pem" key for ssh login in my system. If you don't have then put here.

# Step - 3 Create Dynamic Inventory -
## Dynamic Inventory Directory -(In my case "inventory")

## ``To create dynamic inventory download a program from this link for ec2 -``

https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory

# Command for this -

```"wget https://github.com/ansible/ansible/tree/stable-2.9/contrib/inventory"
```
![image](https://user-images.githubusercontent.com/61896468/97164557-513f9c80-17a8-11eb-873e-8f344aeb4d0a.png) 
# And change python interpreter to "#!/usr/bin/python3" -

![image](https://user-images.githubusercontent.com/61896468/97164512-3f5df980-17a8-11eb-9685-8ef8873646fb.png) 
# Now set these environment variable - "AWS_REGION " , "AWS_ACCESS_KEY_ID" , "AWS_SECRET_ACCESS_KEY"

![image](https://user-images.githubusercontent.com/61896468/97164460-2bb29300-17a8-11eb-8aa8-65a62729325f.png) 
# To check your inventory is working fine then run this command "ansible all --list-hosts" - (I don't have any instance at this time)

![image](https://user-images.githubusercontent.com/61896468/97164415-19d0f000-17a8-11eb-81ac-7a8086852cfb.png) 
## Now It's working fine.

# Step -4 Setup Ansible Role For Launch AWS EC2 Instance-
# (A) Create Role -

## Create Ansible role for ec2 instance with "ansible-galaxy init ec2" . In my case my role name is "ec2".

![image](https://user-images.githubusercontent.com/61896468/97164371-0887e380-17a8-11eb-8cb4-29116834810e.png) 
# (B) Task for launch ec2 instance -

## Task -

![image](https://user-images.githubusercontent.com/61896468/97164323-f60daa00-17a7-11eb-9ef0-f989fe63fff4.png) 
## Write variable for this task in "vars" directory (file "launch.yml") -

## *Security Group is allowing 22 , 80 , 443 , 85.*

![image](https://user-images.githubusercontent.com/61896468/97164257-df675300-17a7-11eb-92a1-5f568c4e7d26.png) 


## Write a Vault "credential.yml" for access key and secret key in vars directory -

![image](https://user-images.githubusercontent.com/61896468/97164200-c65ea200-17a7-11eb-8448-2588cff6bd5d.png)
![image](https://user-images.githubusercontent.com/61896468/97164211-cc548300-17a7-11eb-9e3e-d900a8f0e02a.png)
# (C) Task for Refresh Dynamic Inventory -

## To change in initially loaded inventory we have to reload or refresh that inventory . For this "meta" module is available. To more about this module visit this link -

```
meta – Execute Ansible ‘actions’ ¶

" docs.ansible.com"

This module takes a free form command, as a string. There is not an actual option named "free form". See the examples! flush_handlers makes Ansible run any handler tasks which have thus far been notified. Ansible inserts these tasks internally at...(go through above link for reference) 
```
![image](https://user-images.githubusercontent.com/61896468/97164070-97e0c700-17a7-11eb-977a-cf3138bd6012.png) 

# Step - 5 Setup Ansible Role for Configuring Apache Web Server -
# (A) Create Ansible Role

## Create Role with "ansible-galaxy init webserver" . In my case Role is "webserver"

![image](https://user-images.githubusercontent.com/61896468/97164007-7f70ac80-17a7-11eb-8c04-bdb700ca54ce.png) 
# (B) Task for install Apache web server -

![image](https://user-images.githubusercontent.com/61896468/97163948-6cf67300-17a7-11eb-9eee-cf63427faf00.png) 
## Write variable in "vars" directory in "main.yml" -

![image](https://user-images.githubusercontent.com/61896468/97163908-5c45fd00-17a7-11eb-8bd3-3f2518f6a54b.png) 
# (C) Task For Copying Web Configuration file -

![image](https://user-images.githubusercontent.com/61896468/97163874-49cbc380-17a7-11eb-8048-3dd11593b081.png) 
# I have web configuration file ("web.conf.j2") in "templates" directory

![image](https://user-images.githubusercontent.com/61896468/97163825-36205d00-17a7-11eb-8d97-23e452cb7adb.png) 
## Write Variable in "vars" directory in "main.yml" file -

![image](https://user-images.githubusercontent.com/61896468/97163772-2274f680-17a7-11eb-8ef2-ba85c9ecc52d.png) 


# `(D) Task for Creating DocumentRoot directory -`

![image](https://user-images.githubusercontent.com/61896468/97163705-0d986300-17a7-11eb-8046-de7ff306b8c7.png) 
Variable is already in vars/main.yml file.

# (E) Task For Download web file in DocumentRoot -

![image](https://user-images.githubusercontent.com/61896468/97163661-fb1e2980-17a6-11eb-9c34-fbd9cbba9f72.png) 
## Write variable in vars/main.yml file -

![image](https://user-images.githubusercontent.com/61896468/97163598-e6da2c80-17a6-11eb-9373-c52175a9b46b.png) 


# (F) Task to start httpd service -

![image](https://user-images.githubusercontent.com/61896468/97163502-c1e5b980-17a6-11eb-8aec-2bf02a052e67.png) 
# (G) Handler to restart httpd service -

## Create handler to run it when anything changed in web.conf.j2 file. First For notify we have to use "notify" keyword in "Configure Web Server" task

![image](https://user-images.githubusercontent.com/61896468/97163452-ac708f80-17a6-11eb-993b-4725501ad996.png) 
## "Restart Web Service" is name of handler.

# Write handler in handler directory -

![image](https://user-images.githubusercontent.com/61896468/97163402-995dbf80-17a6-11eb-8c41-05c3dd14e166.png) 
# Step - 6 Create A Playbook To Run "ec2" and "webserver" Ansible Role -
# (A) Create Playbook -

![image](https://user-images.githubusercontent.com/61896468/97163350-86e38600-17a6-11eb-858f-f14ffcd0bfc2.png) 
# (B) Set role path in ansible configuration file -

![image](https://user-images.githubusercontent.com/61896468/97163275-6c111180-17a6-11eb-99e2-305dbdbe8d92.png)
![image](https://user-images.githubusercontent.com/61896468/97163288-716e5c00-17a6-11eb-8bc6-b87f43cd1b20.png)
# Run the "deploy.yml" playbook -

![image](https://user-images.githubusercontent.com/61896468/97163221-4e43ac80-17a6-11eb-9456-f6bc59b53814.png)
![image](https://user-images.githubusercontent.com/61896468/97163240-556aba80-17a6-11eb-81ba-a1cc6fd8c22f.png)
# *Now its working fine -*

![image](https://user-images.githubusercontent.com/61896468/97163169-38ce8280-17a6-11eb-9004-f6627258f11d.png) 
# `Our task is successfully completed.`
