# PAAC – CI/CD Project (Jenkins + SonarQube + Nexus + Docker + AWS ECR/ECS)

This project demonstrates complete CI/CD workflows for the `vprofile` web application using Jenkins.

---

## 🎯 CI/CD Pipelines Implemented

We have **two separate Jenkins pipelines**:

| Pipeline | Output | Target | Jenkinsfile Location |
|---------|--------|--------|---------------------|
| **Pipeline 1:** WAR Artifact Build & Upload | `vprofile-v2.war` | Nexus3 Repository | `jenkins/pipeline-nexus.groovy` |
| **Pipeline 2:** Docker Image build & Deployment | Docker Image | AWS ECR + ECS | `jenkins/pipeline-ecs.groovy` |

---

## 🔁 Pipeline 1 — Jenkins → Maven → SonarQube → Nexus

### Goal
Compile Java code, run tests, ensure code quality, upload WAR artifact to Nexus repository.

### Tools Used
- Jenkins
- Maven + JDK17
- Checkstyle + Jacoco
- SonarQube analysis
- Nexus 3 Artifact Repository

### Artifact Deployment
- WAR file is uploaded to Nexus repo:  
  `http://<NEXUS-IP>:8081/repository/vprofile-repo/`

---

##  Pipeline 2 — Jenkins → Docker → AWS ECR → ECS Deployment

### Goal
Package the web app as a Docker image, push to AWS ECR, trigger rolling deployment on ECS.

### Key Components
| Service | Technology |
|--------|------------|
| Container Registry | AWS ECR |
| Container Runtime | AWS ECS (EC2/FARGATE) |
| Load Balancing (future enhancement) | ALB / NLB |
| Application Runtime | Tomcat on Multi-stage Docker Image |

### Deployment Approach
`aws ecs update-service` triggers **rolling deployment** with **zero downtime**.

---

## 🧱 Multi-stage Dockerfile (Used for Pipeline 2)

Advantages:
- Smaller image size
- Maven not included in final runtime image
- Faster deployments

**Build Stage**
- eclipse-temurin:8-jdk
- Maven runs build → generates WAR

**Runtime Stage**
- Tomcat 8.5
- Deploy WAR as ROOT.war

---


## 🧑‍💻 Author

**Mugilan S**  
DevOps Engineer  
Tools: Jenkins, Docker, AWS, SonarQube, Nexus, GitHub, Linux  
