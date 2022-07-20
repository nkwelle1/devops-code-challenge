# Overview
This repository contains a React frontend, and an Express backend that the frontend connects to.

# Objective
Deploy the frontend and backend to somewhere publicly accessible over the internet. The AWS Free Tier should be more than sufficient to run this project, but you may use any platform and tooling you'd like for your solution.

Fork this repo as a base. You may change any code in this repository to suit the infrastructure you build in this code challenge.

# Submission
1. A github repo that has been forked from this repo with all your code.
2. Modify this README file with instructions for:
* Any tools needed to deploy your infrastructure
* All the steps needed to repeat your deployment process
* URLs to the your deployed frontend.

# Evaluation
You will be evaluated on the ease to replicate your infrastructure. This is a combination of quality of the instructions, as well as any scripts to automate the overall setup process.

# Setup your environment
Install nodejs. Binaries and installers can be found on nodejs.org.
https://nodejs.org/en/download/

For macOS or Linux, Nodejs can usually be found in your preferred package manager.
https://nodejs.org/en/download/package-manager/

Depending on the Linux distribution, the Node Package Manager `npm` may need to be installed separately.

# Running the project
The backend and the frontend will need to run on separate processes. The backend should be started first.
```
cd backend
npm ci
npm start
```
The backend should response to a GET request on `localhost:8080`.

With the backend started, the frontend can be started.
```
cd frontend
npm ci
npm start
```
The frontend can be accessed at `localhost:3000`. If the frontend successfully connects to the backend, a message saying "SUCCESS" followed by a guid should be displayed on the screen.  If the connection failed, an error message will be displayed on the screen.

# Configuration
The frontend has a configuration file at `frontend/src/config.js` that defines the URL to call the backend. This URL is used on `frontend/src/App.js#12`, where the front end will make the GET call during the initial load of the page.

The backend has a configuration file at `backend/config.js` that defines the host that the frontend will be calling from. This URL is used in the `Access-Control-Allow-Origin` CORS header, read in `backend/index.js#14`

# Optional Extras
The core requirement for this challenge is to get the provided application up and running for consumption over the public internet. That being said, there are some opportunities in this code challenge to demonstrate your skill sets that are above and beyond the core requirement.

A few examples of extras for this coding challenge:
1. Dockerizing the application
2. Scripts to set up the infrastructure
3. Providing a pipeline for the application deployment
4. Running the application in a serverless environment

This is not an exhaustive list of extra features that could be added to this code challenge. At the end of the day, this section is for you to demonstrate any skills you want to show that’s not captured in the core requirement.

OPTIONAL EXTRA
README.MD
# Code challenge

## Create a ubuntu server on AWS EC2

Before doing anything we need a server that we can work on, follow these steps to spin up a new Ubuntu 18.04 server instance on AWS EC2.

1. Sign into the AWS Management Console. If you don't have an account yet click the "Create a Free Account" button and follow the prompts.
2. Go to the EC2 Service section.
3. Click the "Launch Instance" button.
4. Choose AMI - Check the "Free tier only" checkbox, enter "Ubuntu" in search box and press enter, then select the "Ubuntu Server 18.04" Amazon Machine Image (AMI).
5. Choose Instance Type - Select the "t2.micro" (Free tier eligible) instance type and click "Configure Security Group" in the top menu.
6. Configure Security Group - Add a new rule to allow HTTP traffic then click "Review and Launch".
7. Review - Click Launch
8. Select "Create a new key pair", enter a name for the key pair (e.g. "my-aws-key") and click "Download Key Pair" to download the private key, you will use this to connect to the server via Putty.
9. Click "Launch Instances", then scroll to the bottom of the page and click "View Instances" to see details of the new Ubuntu EC2 instance that is launching.

## Connect to EC2 via Putty

1. Install Putty
2. With the downloaded key ppk, find the EC2 public IP and login to the instance with username `ubuntu`.

## Deployment

1. Clone the repository in the instance.
2. Install Nodejs with: `sudo apt-get install -y nodejs`.
3. Install nginx with: `sudo apt-get install nginx`.

### Run build

1. After cloning, `cd devops-code-challenge/frontend` and run `npm install` & `npm urn build`.
2. `cd backend` and `npm install`

### Configure nginx

1. Configure nginx for reverse proxy

```bash
    server {
        listen 80 default_server;
        server_name _;

        root /home/ubuntu/devops-code-challenge/frontend/build;

        # react app & front-end files
        location / {
            add_header Access-Control-Allow-Origin *;
            try_files $uri /index.html;
        }
    }
```

Now, open the public IP address of the instance, and we can see that the frontend is served through nginx.

