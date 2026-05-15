name: Push Docker Image to AWS ECR

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v4

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v2

    - name: Build Docker Image
      run: docker build -t docker-repo .

    - name: Tag Docker Image
      run: |
        docker tag docker-repo:latest 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest

    - name: Push Docker Image
      run: |
        docker push 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest
Important Check

Make sure your repository contains:

Dockerfile
Website/application files
Correct folder structure
If Still Failing

Send:

Full GitHub Actions error log
Your Dockerfile
Project folder structure

Then I can identify the exact issue quickly.

give me a docker file

If your project is a simple HTML/CSS/JS website (Coffee Shop project), use this Dockerfile:

# Use Nginx as base image
FROM nginx:latest

# Copy website files into nginx folder
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80
Save File As
Dockerfile

(no extension)

Project Structure Example
project-folder/
│
├── Dockerfile
├── index.html
├── style.css
├── script.js
└── images/
How It Works
Line 1
FROM nginx:latest

Uses Nginx web server.

Line 2
COPY . /usr/share/nginx/html

Copies all website files into Nginx default web folder.

Line 3
EXPOSE 80

Opens port 80 for website access.

Test Locally

Run these commands:

docker build -t coffee-shop .
docker run -d -p 80:80 coffee-shop

Now open:

http://localhost
If You Are Using React / Node.js

Use this instead:

FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
For Your AWS ECR Workflow

After adding Dockerfile:

git add .
git commit -m "Added Dockerfile"
git push

GitHub Actions will automatically:

Build image
Push to ECR
Store image in AWS ECR repository
main_carwebsite.yml
File
add docker file in this file
cafe-menu-main 2.zip
Zip Archive
add docker file in this 

I added the Dockerfile to your project and created an updated ZIP file.

Download the updated project ZIP 

Error: Process completed with exit code 1.

This means the GitHub Action is still failing, but the line:

Process completed with exit code 1

is only the final failure message — not the real error.

You need to check the log lines ABOVE it.

Most Likely Issue

Since your Dockerfile is now correct, the common problem is:

Wrong Workflow File Location

Your workflow YAML must be inside:

.github/workflows/

Example:

.github/workflows/main_carwebsite.yml
Correct Project Structure

Your project should look like this:

cafe-menu-main/
│
├── Dockerfile
├── index.html
├── css/
├── js/
├── images/
│
└── .github/
    └── workflows/
        └── main_carwebsite.yml
Correct Dockerfile

Use:

FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80
Correct YAML Workflow

Use this exact workflow:

name: Push Docker Image to AWS ECR

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:

      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build Docker Image
        run: docker build -t docker-repo .

      - name: Tag Docker Image
        run: |
          docker tag docker-repo:latest 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest

      - name: Push Docker Image
        run: |
          docker push 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest
Important GitHub Secrets

Go to:

GitHub Repo
→ Settings
→ Secrets and variables
→ Actions

Add:

Secret Name	Value
AWS_ACCESS_KEY_ID	Your AWS key
AWS_SECRET_ACCESS_KEY	Your AWS secret
Most Common Causes
Problem	Solution
Dockerfile missing	Add Dockerfile
Wrong workflow location	Put inside .github/workflows/
ECR permissions missing	Add ECR IAM policy
Wrong AWS region	Use same ECR region
Repository doesn't exist	Create docker-repo in ECR
Wrong account ID	Replace with correct AWS account
To Identify Exact Error

Open GitHub Actions logs and copy:

The 15–20 lines ABOVE:
Process completed with exit code 1

That contains the actual error.

Error: Process completed with exit code 1.

Process completed with exit code 1 is only the final message.
The actual error is shown above that line in GitHub Actions logs.

Since your Dockerfile is already added, the failure is usually one of these:

1. Dockerfile Name Is Wrong

Make sure the file name is exactly:

Dockerfile

NOT:

Dockerfile.txt
dockerfile
DockerFile
2. Dockerfile Location Is Wrong

Dockerfile must be in the project root.

Correct:

project/
├── Dockerfile
├── index.html
└── .github/

Wrong:

project/docker/Dockerfile
3. Workflow File Location

Your YAML file must be inside:

.github/workflows/main_carwebsite.yml
4. Use This Exact Dockerfile
FROM nginx:latest

COPY . /usr/share/nginx/html

EXPOSE 80
5. Use This Exact Build Step

Inside YAML:

- name: Build Docker Image
  run: docker build -t docker-repo .

The dot (.) is required.

6. Check AWS ECR Repository

Make sure repository exists:

docker-repo

inside:

AWS → ECR
7. Add ECR Permissions

IAM user needs:

AmazonEC2ContainerRegistryFullAccess
8. Correct Full YAML

Use this exact workflow:

name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Login to Amazon ECR
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build Docker Image
        run: docker build -t docker-repo .

      - name: Tag Docker Image
        run: |
          docker tag docker-repo:latest 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest

      - name: Push Docker Image
        run: |
          docker push 431684229863.dkr.ecr.us-east-1.amazonaws.com/docker-repo:latest
9. Push Again

Run:

git add .
git commit -m "fixed docker workflow"
git push
10. If It Still Fails

Open:

GitHub → Actions → Failed Job

Copy the lines ABOVE:

Process completed with exit code 1

That will show the real issue.
