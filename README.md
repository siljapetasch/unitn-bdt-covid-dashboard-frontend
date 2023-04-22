# Covid Dashboard: A Big Data Technologies University Project

The Covid Dashboard is a frontend application for our Big Data Technologies University project. It provides a visual representation of Covid-19 data, specifically key performance indicators (KPIs) and numbers for different regions in Italy.

## Main Features

- Display Covid-19 data (KPIs and numbers) for different regions in Italy
- Interactive and user-friendly interface

## Technologies and Languages

- Python
- AWS CLI
- Docker

# Frontend

## Build

To build the frontend, navigate to the `dash-frontend` directory and build the Docker image:

```bash
cd dash-frontend
docker build -t dash-frontend .
```

## Run

To run the frontend using Docker, execute the following command:

```bash
docker run -p 8050:8050 dash-frontend
```

## Run Locally

To run the frontend locally without Docker, execute the following command:

```bash
python3 ./app/app.py
```

# AWS ECS Deployment

This section explains how to deploy the Covid Dashboard using a Docker image on AWS Elastic Container Service (ECS).

## Prerequisites

Ensure you have the following tools installed:

- Docker
- Python 3
- AWS CLI

If necessary, configure the AWS CLI with `aws configure`.

## Steps to Deploy

1. Clone the repository:

   ```bash
   git clone https://github.com/siljapetasch/unitn-bdt-covid-dashboard-frontend
   cd unitn-bdt-covid-dashboard-frontend
   ```

2. Build the Docker image locally and test:

   ```bash
   docker build -t docker-dash .
   docker run -p 8050:8050 docker-dash
   ```

3. Send the image to AWS Elastic Container Registry (ECR):

   - Create a repository in AWS ECR:

     ```bash
     aws ecr create-repository --repository-name <my-repo-name>
     ```

   - Get a login authentication token:

     ```bash
     aws ecr get-login --region us-east-1 --no-include-email
     ```

     Copy and paste the output of the above command back into the terminal. You're authenticated locally for 12 hours.

   - Tag the image to the ECR repository:

     ```bash
     docker tag docker-dash:latest ACCT_ID.dkr.ecr.us-east-1.amazonaws.com/<my-repo-name>
     ```

   - Push the image to AWS ECR:

     ```bash
     docker push ACCT_ID.dkr.ecr.us-east-1.amazonaws.com/<my-repo-name>
     ```

4. Deploy the Dash app with ECS (Elastic Container Service):

   Follow the instructions in step 4 here: https://linuxacademy.com/blog/linux-academy/deploying-a-containerized-flask-application-with-aws-ecs-and-docker/

   While doing this, it's easier to change the names to match `<my-thing>-service`, `<my-thing>-cluster`, etc., to make it consistent to refer back to later.

## Updating the Application

If you need to make changes later, you need to build, test, log in (if the token has expired), tag, and push the image to ECR. Then run the following command in the terminal:

```bash
aws ecs update-service --cluster <cluster name> --service <service name> --force-new-deployment
```

This will force the service to reload with the new code.
