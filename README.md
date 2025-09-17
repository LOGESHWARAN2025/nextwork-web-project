# 🚀 Full Stack CI/CD Pipeline using AWS DevOps & GitHub

This project showcases how to set up a fully automated **CI/CD pipeline** using AWS DevOps services and GitHub to build, test, package, and deploy a Java web application from source to production.

---

## 📚 Table of Contents

- [📁 Project Overview](#-project-overview)
- [🛠️ Tools Used](#️-tools-used)
- [📦 AWS Services Breakdown](#-aws-services-breakdown)
- [🧱 Project Structure](#-project-structure)
- [⚙️ Workflow](#️-workflow)
- [🧪 Testing & Troubleshooting](#-testing--troubleshooting)
- [📈 Lessons Learned](#-lessons-learned)
- [✍️ Author](#-author)
- [📌 Project Timeline](#-project-timeline)

---

## 📁 Project Overview

This end-to-end project automates the full software delivery process using AWS DevOps services. The pipeline automatically pulls source code from GitHub, builds and packages it using Maven, stores it in an S3 bucket, and deploys it to an EC2 instance using AWS CodeDeploy.

The web app is developed in Java using Maven and hosted on Apache Tomcat in a Linux EC2 environment.

---

## 🛠️ Tools Used

### 🔗 GitHub
- Source code version control
- Trigger for the pipeline via webhook
- Personal Access Token (PAT) for secure authentication

### 💻 VS Code
- Remote-SSH extension used to connect and work with EC2 instances
- Used as the main IDE for development and configuration

---

## 📦 AWS Services Breakdown

### 🧾 **AWS IAM**
- Created roles with least-privilege permissions
- Roles assigned to CodeBuild, CodeDeploy, EC2, and CodePipeline
- Used to generate access policies (e.g., `codeartifact-network-consumer-policy`)

---

### 💾 **Amazon S3**
- Stores build artifacts (WAR files) from CodeBuild
- Serves as the revision source for CodeDeploy
- Buckets are configured with appropriate access controls

---

### 🏗️ **AWS CodeBuild**
- **Build Automation Tool**: Compiles Java source code, runs unit tests, and packages the WAR file.
- **buildspec.yml**: Defines commands to run during install, pre_build, build, and post_build phases.
- Pulls dependencies from CodeArtifact using `settings.xml`.

---

### 📦 **AWS CodeArtifact**
- Used as a secure and scalable artifact repository.
- Integrated with Maven via `settings.xml` to pull third-party dependencies.
- Organized under a domain (e.g., `nextwork`) and supports upstream repositories like Maven Central.

---

### 🚀 **AWS CodeDeploy**
- **Deployment Tool**: Deploys built application to EC2 instances.
- **appspec.yml**: Contains hooks that run custom scripts like `install_dependencies.sh`, `start_server.sh`, and `stop_server.sh`.
- Handles deployment lifecycle stages such as install, start, and stop.

---

### 🔄 **AWS CodePipeline**
- **CI/CD Orchestration Tool**: Automates the pipeline from source (GitHub) to deploy (EC2).
- Connected with GitHub using AWS CodeConnections.
- Triggers build and deploy stages on every code commit.

---

### ☁️ **AWS EC2**
- Hosts the web application.
- Separate development and production environments created via CloudFormation.
- Instances configured with Apache Tomcat and required software using Bash scripts.

---

### 🧰 **AWS CloudFormation**
- Automates infrastructure provisioning.
- Used to create networking components (VPC, subnets) and EC2 instances.
- Enables quick teardown and rebuilding of production environments.

---

## 🧱 Project Structure

# My Web Application

This repository contains a web application deployed with **AWS CodePipeline**, **CodeBuild**, and **CodeDeploy**.  

## Project Structure

```bash
.
├── index.jsp                 # Web app UI code
├── settings.xml              # Maven & CodeArtifact configuration
├── buildspec.yml             # CodeBuild configuration
├── appspec.yml               # CodeDeploy instructions
├── scripts/
│   ├── install_dependencies.sh
│   ├── start_server.sh
│   └── stop_server.sh

#end

---

## ⚙️ Workflow

1. **Development & Version Control**
   - Code written in Java using VS Code and Maven.
   - Source code managed in GitHub with branches and commit history.

2. **CI/CD Pipeline (AWS CodePipeline)**
   - **Source Stage**: GitHub repo integrated via CodeConnections.
   - **Build Stage**: CodeBuild builds the app using `buildspec.yml`.
   - **Deploy Stage**: CodeDeploy uses `appspec.yml` to deploy to EC2.

3. **Infrastructure**
   - EC2 instances for production created with CloudFormation.
   - IAM policies give necessary service permissions.
   - S3 stores built WAR files.

4. **Artifact Management**
   - CodeArtifact used for secure Maven dependency management.
   - Maven config (`settings.xml`) includes authentication tokens for repository access.

---
## 📦 Key Features

### 🟢 GitHub Integration
- Initialized Git repo on EC2 using `git init`
- Connected local repo to GitHub using Personal Access Token (PAT)
- Used `git add`, `commit`, `push` to track and update changes

### 🏗️ Build Automation with CodeBuild
- `buildspec.yml` defines install, pre_build, build, and post_build phases
- WAR file created and uploaded to S3 bucket

### 🔐 Secure Packaging with CodeArtifact
- Maven configured via `settings.xml` to fetch and store packages
- IAM policy created to allow EC2 and CodeBuild to access CodeArtifact

### 🚀 Deployment using CodeDeploy
- Bash scripts for dependency install, server start/stop
- `appspec.yml` for lifecycle hooks
- Deployed to EC2 instance provisioned by CloudFormation

### 🔄 CI/CD Automation with CodePipeline
- Fully automated flow: **Source (GitHub) → Build (CodeBuild) → Deploy (CodeDeploy)**
- Webhooks used to trigger pipeline on code push

---

## 🌐 Hosting & Access

- Application deployed to EC2
- Accessed using **EC2 Public IPv4 DNS** via web browser (HTTP)

---

## 🧪 Testing & Rollback

- Monitored build/deploy logs in **CloudWatch**
- Tested CodeDeploy rollback by introducing a failed deployment
- Rollback triggered successfully using `AllAtOnce` deployment config

---

## 🧪 Testing & Troubleshooting

- **Build Debugging**: Monitored logs in CodeBuild and CloudWatch.
- **Deployment Failures**: Analyzed CodeDeploy logs; fixed issues like misplaced `appspec.yml`.
- **Rollback Testing**: Introduced intentional error (e.g., typo in Bash script) to simulate failure and verify rollback behavior.

---

## 📈 Lessons Learned

- CodeDeploy requires `appspec.yml` in the root directory.
- GitHub now mandates **Personal Access Tokens** instead of passwords.
- CodeArtifact needs properly scoped IAM policies for access.
- CodePipeline stages can be monitored and debugged individually.

---

## ✍️ Author

**Logeshwaran R**  
📧 logeshrajkumar2025@gmail.com

---

## 📌 Project Timeline

| Module                         | Duration      |
|-------------------------------|---------------|
| VS Code + EC2 Setup           | 2 hours       |
| GitHub Integration            | 3.5 hours     |
| CodeArtifact Configuration    | 2 hours       |
| CodeBuild Setup               | 2 hours       |
| CodeDeploy + EC2 Automation   | 4.5 hours     |
| CodePipeline Automation       | 2 hours       |
| Documentation                 | 1 hour        |
| **Total Time**                | **~15 hours** |

---

## ⭐️ Show Your Support

If you like this project or learned something from it:

- ⭐ Star this repo
- 🍴 Fork it
- 📣 Share with others

---

## 🔗 Related Projects

This project is part of a 5-part DevOps series:
1. [✅ Setup EC2 + VS Code + Java](#)
2. [✅ GitHub Integration](#)
3. [✅ CodeArtifact & Maven](#)
4. [✅ CodeBuild & CI Setup](#)
5. [✅ CodeDeploy & Full CI/CD](#)

---

