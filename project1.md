# Step 0: Creating Pre-Requisites 

Created an AWS account: Here is a screenshot of my AWS homepage: 

![awshomescreen](https://user-images.githubusercontent.com/110178748/182002150-adf19f74-b49e-4678-9a6a-6a63bad4ef0f.png)

# Created a New EC2 Instance called andrewpbl2


![EC2instace](https://user-images.githubusercontent.com/110178748/182002994-c16eaa29-6a4a-4163-b09b-28fd9ccae349.png)


# Connected andrewpbl2 instance using PEM key by running the following command *sh -i andrewpbl1.pem ubuntu@3.86.87.66*


![connecttoec2instance](https://user-images.githubusercontent.com/110178748/182003018-afa69f8c-0ee2-40be-b830-d6eafa29c9c8.png)

# Configuring the EC2 machine to serve a Web server

## Step 1: Installing Apache and Updating the Firewall 

### Installing Apache using apt

update a list of packages in package manager

sudo apt update 

![sudo apt update](https://user-images.githubusercontent.com/110178748/182156847-257add1c-7b05-40ea-bdc1-ba51c67d76f2.png)


run apache2 package installation

sudo apt install apache2

![sudo apt install apache2](https://user-images.githubusercontent.com/110178748/182157547-e656b7bf-4be3-4595-b551-f3bb5a05c637.png)


To verify that apache2 is running as a Service in our OS I ran 

sudo systemctl status apache2

![sudo systemctl status apache2](https://user-images.githubusercontent.com/110178748/182157928-c09a2540-18d5-4672-bfde-003fecd97bf9.png)


opening TCP port 80, the default port for web browsers to access web pages on the Internet

TCP port 22 open by default on my EC2 machine to access it via SSH, so I added a rule to EC2 configuration to open inbound connection through port 80

![inbound rules edit of ec2instance](https://user-images.githubusercontent.com/110178748/182159193-bc29921e-28c5-47c8-889b-61163ea9f1c9.png)


![inbound rules functioning confirmation](https://user-images.githubusercontent.com/110178748/182159374-c40c4f71-d356-42ed-a9d9-bdc70cf8b090.png)


I now Check how I can access it locally in my Ubuntu shell,

I ran :  curl http://localhost:80 





