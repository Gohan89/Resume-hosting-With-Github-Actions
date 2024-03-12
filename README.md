Setting Up GitHub Actions for EC2 Deployment:
Step 1: Prepare Your EC2 Instance

Create an EC2 instance on AWS.
Download the private key pair (.pem file) associated with the instance. You'll use this for SSH access.
Step 2: Create GitHub Secrets

In your GitHub repository settings, navigate to "Secrets".
Create secrets for the following:
EC2_SSH_KEY: Paste the contents of your downloaded .pem file here (securely encrypted).
HOST_DNS: Enter the public DNS name of your EC2 instance (e.g., ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com).
USERNAME: Specify the username for your EC2 instance (often ubuntu).
TARGET_DIR: Define the directory on your EC2 instance where you want to deploy your code.
Step 3: Create Your Workflow File

In your GitHub repository, create a directory named .github/workflows if it doesn't exist.
Inside the .github/workflows directory, create a new file named github-actions-ec2.yml.
Step 4: Define Your Workflow Steps

Edit the github-actions-ec2.yml file to define your deployment workflow. Here's a basic example:
YAML


name: Deploy to EC2

on: push:
  branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    - name: Checkout files
      uses: actions/checkout@v2

    - name: Deploy to server
      uses: easingthemes/ssh-deploy@main  # Or another SSH action
      env:
        SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
        REMOTE_HOST: ${{ secrets.HOST_DNS }}
        REMOTE_USER: ${{ secrets.USERNAME }}
        TARGET: ${{ secrets.TARGET_DIR }}

        
Use code with caution.
Important Notes:

Replace the placeholders (easingthemes/ssh-deploy@main) with the specific SSH action you choose.

Make sure to indent the steps correctly (two spaces) within the jobs section.

Remember to encrypt your EC2_SSH_KEY secret in GitHub settings.

Step 5: Test Your Workflow

Push your changes to your GitHub repository.
The workflow should automatically trigger and attempt deployment upon pushing to the specified branch (e.g., main).
Check the "Actions" tab in your repository for workflow execution logs.
