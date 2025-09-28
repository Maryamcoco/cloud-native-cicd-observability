---
# Cloud Native CI/CD Observability 🚀

This project demonstrates a full **end-to-end DevOps** pipeline for deploying a **Java** application using modern **CI/CD** and observability practices. It integrates infrastructure automation, containerization, continuous integration, continuous delivery, and monitoring using industry-standard tools.

---
## 📌 Project Overview

**Application:** Java app (built with Maven)

**CI/CD Orchestration:** Jenkins

**Containerization:** Docker

**Container Orchestration:** Kubernetes (EKS)

**Infrastructure as Code:** Terraform

**Image Registry:** AWS Elastic Container Registry (ECR)

**Code Quality & Security:** SonarCloud

**Monitoring & Observability:**

- Datadog (via Datadog Agent installed on EC2 Linux)

- Prometheus & Grafana (for metrics and dashboards)

**Infrastructure as Code (IaC)** with Terraform (separate setup, not included in this repo)

---

**NOTE:** This repository contains the application source code and Jenkins pipeline configuration. Infrastructure resources were provisioned using Terraform (managed outside this repo).


## 🛠️ Tools & Services
- **Jenkins**

  - *Installed ```Maven 3.8``` as a Jenkins tool.*
  - *Installed the required plugins:*
    - Pipeline stage view
    - Prometheus metrics plugin
    - Docker pipeline
    - AWs credentials
    - Amazon ECR
    - Kubernetes CLI
  
  - *Configured credentials:*

    - SonarCloud Token
    - DockerHub Username & Password
    - Kubernetes ```kubeconfig``` content
    - AWS IAM Access Keys

- **DockerHub** – hosted container images for deployments.

- **AWS ECR** – cluster-specific image storage.

- **Terraform** – provisioned EC2, EKS, networking, IAM roles, etc.

- **SonarCloud** – static code analysis.

- **Prometheus & Grafana** – observability (metrics scraping & visualization).

- **Datadog – observability** - (agent installed on EC2 to collect telemetry & send to Datadog dashboards).
--- 
## ⚙️ CI/CD Pipeline Flow

**1. Code Commit →** 
   - Developer pushes to GitHub.

**2. Jenkins Build →**

   - Pulls source code.

   - Runs Maven build & tests.

   - Performs SonarCloud analysis.

   - Builds Docker image.

   - Pushes image to DockerHub & AWS ECR.

**3. Deployment to Kubernetes (EKS) →**

   - Uses kubeconfig credentials.

   - Deploys updated Docker image.

**4. Monitoring →**

   - Prometheus scrapes metrics.

   - Grafana dashboards display system health.
  
   - Datadog agent collects and forwards logs/metrics/traces.


## 📊 Observability & Monitoring
 #### Datadog

  - Installed **Datadog-Agent** on EC2 Linux instance
  - enable **datadog.yaml** file
  - restart the **datadog-agent**
  - Collected system & application metrics
  - Integrated dashboards (CPU, Memory, Docker, Kubernetes)

 #### Prometheus

  - Installed via Terraform & Helm
   - Scraping metrics from Node Exporter & Blackbox Exporter

 #### Grafana
  - Connected to Prometheus as data source
  - Dashboards created for Kubernetes workloads & app performance


## 📂 Repository Structure
```
cloud-native-cicd-observability

│── src/                   # Java application source code
│── .gitignore             # files to ignore
│── Dockerfile             # Container image definition
│── Jenkinsfile            # CI/CD pipeline definition
│── License                # Apache License 2.0 for this project
│── README.md              # Project documentation
│── Catalina.policy        # controls permissions for Java code running in Tomcat
│── Deployment.yaml        # kubernetes deployment definition defines how  app is deployed on a cluster
│── Pom.xml                # defines dependencies, build plugins, and project structure
```
**Note:** Terraform infrastructure code is managed separately and not included in this repo.

## 📸 Screenshots & Recordings
### Home Page
![Home Page](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/application%20homepage.png)
*Home page of the application*

### Jenkins pipeline stages
![jenkins pipeline stage view](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/Jenkins%20pipeline%20view.png)

### SonarCloud Analysis Dashboard
![Sonarcloud dashboard](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/sonarcloud%20analysis%20dashboard.png)

### Grafana node exporter dashboard 
![Node Exporter Dashboard](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/node%20exporter%20dashboard%20on%20grafana.png)

### Grafana dashboard full 
![Node Exporter Dashboard](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/node%20exporter%20full.png)

### Datadog Metrics
![Datadog Metrics](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/datadog%20dashboard.png)

### Datadog Dashboard
![Datadog infrastructure host](https://github.com/Maryamcoco/cloud-native-cicd-observability/blob/master/media/Infrastructure%20list%20on%20Datadog.png)


## ⚙️ How to Use
#### 🔹 Method 1 (Recommended): Run via Jenkins CI/CD

1. **Fork/Clone this repo**
```
git clone https://github.com/Maryamcoco/cloud-native-cicd-observability.git
cd cloud-native-cicd-observability
```

2. **Create a Jenkins Pipeline** job pointing to this repo.

3. **Install the necesary plugins** 

4. Configure **credentials & tools** in Jenkins (see list above).

5. **Run the pipeline** → Jenkins automates build, test, image push, and deployment.

#### 🔹 Method 2 (Optional): Run Manually

* If you want to run the app locally without Jenkins😄

3. Clone repo & build:
```
mvn clean install
```

4. Build Docker image:
```
docker build -t your-dockerhub-username/java-app .
```

5. Push to DockerHub:
```
docker push your-dockerhub-username/java-app
```

6. Deploy to Kubernetes:
```
kubectl apply -f k8s-deployment.yaml
```

## 📜 License
Copyright (c) 2025 **Maryam Abdulrauf**😄
Licensed under the **Apache License**, Version 2.0

