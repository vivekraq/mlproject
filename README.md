# Student Performance Prediction | End-to-End ML & AWS CI/CD

An end-to-end Machine Learning project that predicts student performance using a trained ML model, Flask web application, Docker containerization, and AWS CI/CD services.

The project demonstrates how to organize an ML application, package it into a Docker image, and automate the build and image publishing process using AWS developer tools.

## Project Overview

This project focuses on building a Student Performance Prediction system with a modular machine learning pipeline and a web-based interface.

### Key Features

- Data ingestion and preprocessing
- Exploratory Data Analysis (EDA)
- Machine Learning model training
- Model serialization using Pickle
- Flask-based web application
- Modular Python project structure
- Docker containerization
- AWS CodePipeline integration with GitHub
- AWS CodeBuild for automated Docker image building
- Amazon ECR for container image storage

## Technology Stack

| Category | Technologies |
|---|---|
| Programming Language | Python |
| Machine Learning | Scikit-learn, CatBoost |
| Backend | Flask |
| Data Processing | Pandas, NumPy |
| Frontend | HTML, CSS, Jinja2 |
| Containerization | Docker |
| CI/CD | AWS CodePipeline, AWS CodeBuild |
| Container Registry | Amazon ECR |
| Version Control | Git, GitHub |
| Cloud Region | AWS Europe (Stockholm) — eu-north-1 |

## Project Structure

```text
mlproject/
│
├── app.py
├── application.py
├── requirements.txt
├── setup.py
│
├── artifacts/
│   ├── model.pkl
│   ├── preprocessor.pkl
│   ├── raw.csv
│   ├── train.csv
│   └── test.csv
│
├── notebook/
│   ├── EDA Student Performance.ipynb
│   └── Model Training.ipynb
│
├── src/
│   ├── components/
│   │   ├── data_ingestion.py
│   │   ├── data_transfomation.py
│   │   └── model_trainer.py
│   │
│   ├── pipeline/
│   │   ├── predict_pipeline.py
│   │   └── train_pipeline.py
│   │
│   ├── exception.py
│   ├── logger.py
│   └── utils.py
│
├── templates/
│   ├── home.html
│   └── index.html
│
├── Dockerfile
└── README.md
```

## Machine Learning Workflow

The project follows a modular ML development process:

1. Data collection and ingestion
2. Exploratory Data Analysis
3. Data preprocessing and transformation
4. Model training
5. Model and preprocessor serialization
6. Prediction through the Flask application

## AWS CI/CD Architecture

The deployment workflow is designed around the following services:

```text
Developer
    |
    v
GitHub Repository
    |
    | Source Changes
    v
AWS CodePipeline
    |
    v
AWS CodeBuild
    |
    | Install Dependencies
    | Authenticate with ECR
    | Build Docker Image
    | Tag Docker Image
    v
Amazon ECR
    |
    v
Container Image
    |
    v
Future Deployment Target
```

## AWS Implementation

### 1. GitHub Source Integration

Connected the GitHub repository to AWS CodePipeline to use the `main` branch as the source for automated builds.

Repository: https://github.com/vivekraq/mlproject

### 2. AWS CodePipeline

Created a pipeline named:

`SimpleDockerService`

The pipeline is configured to retrieve source code from GitHub and initiate the CodeBuild stage.

### 3. AWS CodeBuild

Configured the build project:

`SimpleDockerProject-0a00fe57ed85`

The build process includes:

- Installing project dependencies
- Authenticating with Amazon ECR
- Building the Docker image
- Tagging the image
- Pushing the image to the ECR repository

### 4. Docker Image Build

The Docker image is built using the project Dockerfile.

```bash
docker build -t $ImageName .
```

### 5. Amazon ECR Authentication

Authenticate Docker with Amazon ECR:

```bash
aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.eu-north-1.amazonaws.com
```

### 6. Docker Image Tagging

```bash
docker tag $ImageName:latest <AWS_ACCOUNT_ID>.dkr.ecr.eu-north-1.amazonaws.com/$ImageName:latest
```

### 7. Push Image to Amazon ECR

```bash
docker push <AWS_ACCOUNT_ID>.dkr.ecr.eu-north-1.amazonaws.com/$ImageName:latest
```

AWS Account ID and repository names should be configured through environment variables rather than hardcoded into public source code.

## Build Configuration

The CodeBuild buildspec defines three main phases:

| Phase | Responsibility |
|---|---|
| Pre-build | Authenticate with Amazon ECR |
| Build | Build Docker image |
| Post-build | Tag and push image to ECR |

## Challenges and Troubleshooting

During implementation, I worked through several cloud deployment and configuration challenges:

- AWS CodeBuild account build concurrency quota limitation
- AWS region consistency between CodePipeline, CodeBuild, and ECR
- Docker authentication with Amazon ECR
- ECR repository and image naming
- Missing Dockerfile during the initial build
- Buildspec configuration and Docker image tagging

These issues helped strengthen my understanding of AWS permissions, cloud resource configuration, and CI/CD troubleshooting.

## Learning Outcomes

Through this project, I gained practical experience with:

- Structuring a production-oriented ML application
- Integrating GitHub with AWS CodePipeline
- Automating Docker builds using AWS CodeBuild
- Managing container images with Amazon ECR
- Understanding cloud IAM permissions and regional resources
- Debugging CI/CD pipeline failures

## Future Enhancements

- Complete automated image publishing to Amazon ECR
- Deploy the container to Amazon ECS
- Configure application networking and public access
- Add automated testing to the CI/CD workflow
- Implement monitoring and logging
- Introduce automated model retraining

## Author

**Vivek Rawat**

B.Tech — Computer Science and Engineering

GitHub: https://github.com/vivekraq

LinkedIn: https://www.linkedin.com/in/vivek1717/

---

⭐ If you find this project useful, consider giving the repository a star.
