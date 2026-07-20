# Hosting Nginx on AWS EC2 with CI/CD Pipeline

Hello, this is my project in which I am hosting an nginx server on a cloud server.

## Tools that I'm using
WSL (Ubuntu) {for sshing into my cloud server}
WSL (Local Machine) {for making changes in the files of local machine}
AWS EC2 (t3.micro) {for creating instances of web servers}
AWS S3 {for uploading media content in our server}
AWS IAM {for assigning roles}
Docker
Github Actions {for creating pipeline}

## Architecture
Local Machine → GitHub → GitHub Actions → AWS EC2 → Docker Container → Nginx

First I did setup my very first instance by choosing desired confuguration methods. After that I sshed into the server with the help of a pem key and public IP provided to me. Then I installed nginx in it and hosted an index page on it. I then used S3 and setup that server as a file sharing server for my cloud server. I even uploaded a txt file on it. Later i created IAM roles for creating roles for different persons. I set the role to only view not change for S3. Now I attatched that to S3 and S3 to EC2. Now the thing with our web page is that it was static and in order to make any change we've to make it in the instance itself so I created a folder on local machine and created an index page in it. Also assif=gned an Elastic IP which basically serves as an Static IP for our server Now setup git and created a repository in github. and connected it with my local machine folder and later to my EC2 instance. But still I've to push and pull the code to make any changes to my webpage. In order to automate this I created a Github pipeline by using workflows which automatically sends the data to EC2 server wheb I make any commit. Now I made a docker image and gave the image a tag then pushed it o Docker Hub and then created a fresh EC@ instace and pulled the image from it.

1. Code change made locally
2. Pushed to GitHub main branch
3. GitHub Actions pipeline triggers automatically
4. Pipeline SSHes into EC2 using stored secrets
5. Pulls latest code from GitHub
6. Nginx serves the updated webpage live

## Key Concepts Implemented
- EC2 instance provisioning and configuration
- SSH key based authentication
- Security group firewall rules (ports 22 and 80)
- Elastic IP for static public address
- IAM role with S3 read-only access attached to EC2
- Dockerfile to containerise nginx
- Docker Hub image push and pull
- GitHub Actions workflow with encrypted secrets

## Lesson Learned
- SSH keys must include full header and footer lines 
  when used in CI/CD secrets — missing this broke 
  the pipeline for 30 minutes
- Secret names in GitHub Actions must exactly match 
  what's referenced in the workflow file
- Port 80 must be opened in security group separately 
  from SSH — EC2 blocks all ports by default
- Stopped containers still exist and consume space — 
  always docker rm after docker stop
