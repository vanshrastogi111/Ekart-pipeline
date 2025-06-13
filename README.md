# Ekart

Ekart is a sample e-commerce backend project designed to demonstrate CI/CD best practices using Jenkins, SonarQube, OWASP Dependency Check, and Docker.

## 🚀 Project Overview

This project showcases an automated Jenkins pipeline that:
- Checks out code from GitHub
- Compiles the Java application with Maven
- Performs static code analysis using SonarQube
- Runs OWASP dependency checks for vulnerabilities
- Builds a Docker image and pushes it to Docker Hub
- Deploys the application in a Docker container


## ⚙️ Prerequisites

Ensure the following tools are configured in Jenkins:

- **Maven** (`maven3`)
- **SonarQube Scanner** (`sonar-scanner`)
- **SonarQube Server** (`sonar-server`)
- **OWASP Dependency Check** (`owasp`)
- **Docker** (`docker`)
- **Docker Hub credentials** configured as Jenkins credentials (ID: `34932390-b59c-45d3-b257-54c349106d3a`)

## 🔄 Jenkins Pipeline Stages

1. **Git Checkout**
   - Clones the `main` branch of this repository.

2. **Compile**
   - Compiles the project using `mvn clean compile`.

3. **SonarQube Analysis**
   - Runs static analysis using SonarQube.

4. **Build**
   - Packages the application with Maven, skipping tests.

5. **OWASP Dependency Check**
   - Scans dependencies for known vulnerabilities.

6. **Build Docker Image**
   - Builds a Docker image from `docker/Dockerfile` and tags it.

7. **Push Docker Image**
   - Pushes the Docker image to Docker Hub.

8. **Deploy To Docker Container**
   - Runs the Docker container on port `8083`.

## 🐳 Docker Usage

After the pipeline completes, the app is available as a Docker container:

```bash
docker run -d --name ekart -p 8083:8083 vanshrastogi111/ekart:latest
```
Access the application at: http://localhost:8083

## 📊 SonarQube & Security
SonarQube server analyzes code quality and reports bugs, vulnerabilities, and code smells.

OWASP Dependency Check ensures libraries used are free of known security issues.

## 📦 Maven
To build the project locally:

```bash
mvn clean package
```
