# DevOps CI/CD Automation Project

## Project Overview

This project demonstrates an end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Python Flask web application.

The pipeline automates source code checkout, dependency installation, testing, code quality analysis, Docker image creation, image publishing and deployment to Kubernetes.

## Architecture

GitHub → Jenkins → Pytest → SonarCloud → Docker Build → Docker Hub → Kubernetes

## Technologies Used

- Python 3.9
- Flask
- Pytest
- Pytest Coverage
- Jenkins
- SonarCloud
- Docker
- Docker Hub
- Kubernetes
- Minikube
- Rocky Linux 9.5
- Git

## Application

The application exposes the following endpoints:

| Endpoint | Description |
|---|---|
| `/` | Returns the application welcome message |
| `/health` | Health check endpoint |
| `/version` | Returns the application version |

## CI/CD Pipeline Stages

1. Checkout source code from GitHub.
2. Create a Python virtual environment.
3. Install application and testing dependencies.
4. Execute automated tests using Pytest.
5. Generate code coverage reports.
6. Perform code quality analysis using SonarCloud.
7. Build a Docker image.
8. Push Docker images to Docker Hub.
9. Deploy the latest build to Kubernetes.
10. Verify the Kubernetes deployment rollout.

## Testing

Automated tests are executed using Pytest.

Coverage is generated using pytest-cov.

The project achieved approximately 92% test coverage during testing.

## Docker

The Flask application is containerized using Docker.

Docker images are built using the project Dockerfile and published to Docker Hub.

Docker Hub Repository:

https://hub.docker.com/r/2025tm93032/devops-flask-app

## Kubernetes Deployment

The application is deployed to a Minikube Kubernetes cluster.

Deployment configuration:
- Deployment name: devops-flask-app
- Replicas: 2
- Container port: 5000
- Service name: devops-flask-service
- Service type: NodePort

Kubernetes is used to manage application deployment, replicas and rolling updates.

## Rolling Updates and Rollback

The project demonstrates Kubernetes rolling updates by updating the application deployment with a new Docker image.

Rollback functionality was also tested using Kubernetes rollout history and rollout undo commands.

This allows the application to return to a previous deployment version.

## Application Verification

The application was tested through Kubernetes using port forwarding.

Endpoints tested:

- http://localhost:8081/
- http://localhost:8081/health
- http://localhost:8081/version

## Repository

GitHub:

https://github.com/abhi3627-bits/devops-cicd-project

## Project Outcome

This project demonstrates an automated CI/CD workflow integrating source control, automated testing, code quality analysis, containerization, image publishing and Kubernetes deployment.

The implementation also demonstrates deployment updates, application verification and rollback capabilities.
