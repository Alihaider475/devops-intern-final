# DevOps Intern Final Assessment

## Author

**Ali Haider**

## Date

**8-August-2026**

---

## Project Description

This project demonstrates a basic but realistic DevOps workflow using open-source and industry-relevant tools.

The project covers:

* Git & GitHub for version control
* Linux for system administration
* Docker for containerization
* GitHub Actions for CI/CD
* HashiCorp Nomad for workload orchestration
* Grafana Loki for log aggregation
* Grafana Alloy for collecting and forwarding container logs

The goal is to demonstrate how source code can move through a simple DevOps pipeline from development to containerization, automation, deployment, and monitoring.

---

# Project Architecture

```text
Developer
    |
    v
Git Repository
    |
    v
GitHub
    |
    v
GitHub Actions
    |
    +---- Build & Test
    |
    +---- Docker Build
    |
    v
Docker Image
    |
    v
Nomad
    |
    v
Application Container
    |
    v
Docker Logs
    |
    v
Grafana Alloy
    |
    v
Grafana Loki
    |
    v
Log Queries / Monitoring
```

---

# Step 1: Git & GitHub Setup

A GitHub repository was created to manage the project source code and track changes.

The initial Python application is:

```python
print("Hello, DevOps!")
```

Git was initialized and the project was connected to the GitHub repository.



### Verification

The repository was verified on GitHub and the project files were successfully pushed.

**Screenshot:**

`![GitHub Repository](monitoring/screenshots/github-repository.png)`

---

# Step 2: Linux

Linux was used as the environment for running and managing DevOps services.

The following Linux concepts and commands were used during the project:

```bash
pwd
ls
cd
mkdir
cat
chmod
ps
top
df -h
free -h
systemctl
```

Linux was also used for managing processes, checking system resources, and troubleshooting services.

### Resource Monitoring

Commands such as:

```bash
df -h
free -h
top
```

were used to inspect disk usage, memory usage, and running processes.

**Screenshot:**

`![Linux Environment](monitoring/screenshots/linux.png)`

---

# Step 3: Docker Containerization

Docker was used to package the Python application into a container.

## Dockerfile

The application was containerized using a Dockerfile.

Example:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY app.py .

CMD ["python", "app.py"]
```

### Build Docker Image

```bash
docker build -t devops-intern-app .
```

### Run Container

```bash
docker run --name devops-app devops-intern-app
```

### Verify Container

```bash
docker ps
docker logs devops-app
```

The application output was successfully displayed from the Docker container.

**Screenshot:**

`![Docker Container](monitoring/screenshots/docker.png)`

---

# Step 4: GitHub Actions CI/CD

GitHub Actions was used to automate the CI/CD workflow.

The workflow is triggered when changes are pushed to the GitHub repository.

The pipeline performs tasks such as:

1. Checkout source code
2. Set up the required environment
3. Run application checks
4. Build the Docker image

Example workflow location:

```text
.github/
└── workflows/
    └── ci.yml
```

A simplified workflow structure is:

```yaml
name: CI

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t devops-intern-app .
```

### CI Verification

The GitHub Actions workflow was executed successfully and the Docker image build was validated automatically.

**Screenshot:**

`![GitHub Actions](monitoring/screenshots/github-actions.png)`

---

# Step 5: Nomad Deployment

HashiCorp Nomad was used to demonstrate workload orchestration.

A Nomad job specification was created to define how the application should be deployed.

Example job structure:

```text
nomad/
└── app.nomad
```

The Nomad job defines:

* Application service
* Container image
* Resource requirements
* Network configuration
* Restart behavior

Example commands:

```bash
nomad job validate app.nomad
nomad job run app.nomad
nomad job status
```

The application was deployed as a Nomad workload.

**Screenshot:**

`![Nomad Job](monitoring/screenshots/nomad.png)`

---

# Step 6: Grafana Loki

Grafana Loki was used as the centralized log storage system.

Application/container logs were collected and forwarded to Loki.

The basic logging flow is:

```text
Docker Container
       |
       v
Docker Logs
       |
       v
Grafana Alloy
       |
       v
Grafana Loki
```

Loki provides centralized storage and querying of application logs.

The Loki service was verified using its API.

Example:

```text
http://localhost:3100
```

Logs could then be queried using LogQL.

Example:

```logql
{service_name="unknown_service"}
```

---

# Step 7: Grafana Alloy

Grafana Alloy was used as the log collection and forwarding agent.

Alloy was configured to discover Docker containers and collect their logs.

Configuration location:

```text
monitoring/
└── alloy/
    └── config.alloy
```

The configuration connects the Docker log source to Loki.

The general flow is:

```text
Docker
  |
  | Container logs
  v
Grafana Alloy
  |
  | Log forwarding
  v
Grafana Loki
```

Alloy was run as a Docker container with access to the Docker socket and the required configuration file.

Example verification:

```bash
docker ps
docker logs alloy
```

**Screenshot:**

`![Grafana Alloy](monitoring/screenshots/alloy.png)`

---

# Step 8: Monitoring and Log Verification

The monitoring setup was tested by generating application/container logs and checking whether they were successfully received by Loki.

The monitoring stack consisted of:

| Component     | Purpose                     |
| ------------- | --------------------------- |
| Docker        | Runs application containers |
| Grafana Alloy | Collects and forwards logs  |
| Grafana Loki  | Stores and queries logs     |

Example Loki query:

```logql
{service_name="unknown_service"}
```

The Loki API was also used to verify that logs were being received.

**Screenshot:**

`![Loki Logs](monitoring/screenshots/loki-query.png)`

---

# Project Directory Structure

```text
devops-intern-final/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── monitoring/
│   ├── alloy/
│   │   └── config.alloy
│   │
│   └── screenshots/
│       ├── github-repository.png
│       ├── github-actions.png
│       ├── docker.png
│       ├── nomad.png
│       ├── alloy.png
│       └── loki-query.png
│
├── nomad/
│   └── app.nomad
│
├── Dockerfile
├── app.py
└── README.md
```

---

# How to Run

## 1. Clone the Repository

```bash
git clone <GITHUB_REPOSITORY_URL>
cd devops-intern-final
```

## 2. Run the Application

```bash
python app.py
```

Expected output:

```text
Hello, DevOps!
```

## 3. Build the Docker Image

```bash
docker build -t devops-intern-app .
```

## 4. Run the Docker Container

```bash
docker run --name devops-app devops-intern-app
```

## 5. Check Logs

```bash
docker logs devops-app
```

## 6. Run the Nomad Job

```bash
nomad job validate nomad/app.nomad
nomad job run nomad/app.nomad
```

## 7. Verify Monitoring

Check the Alloy and Loki containers:

```bash
docker ps
```

Then inspect Alloy logs:

```bash
docker logs alloy
```

Loki can be queried to verify that application logs are being received.

---

# DevOps Workflow Summary

The complete workflow implemented in this project is:

```text
Code
 |
 v
Git
 |
 v
GitHub
 |
 v
GitHub Actions
 |
 v
Docker Build
 |
 v
Nomad Deployment
 |
 v
Application
 |
 v
Docker Logs
 |
 v
Grafana Alloy
 |
 v
Grafana Loki
 |
 v
Log Query
```

This demonstrates the core DevOps lifecycle of **version control, automation, containerization, orchestration, and observability**.

---

# Conclusion

This project provided practical experience with a complete DevOps workflow.

The main technologies demonstrated were:

* Git
* GitHub
* Linux
* Docker
* GitHub Actions
* HashiCorp Nomad
* Grafana Alloy
* Grafana Loki

The final workflow demonstrates how application source code can be version-controlled, automatically validated, containerized, deployed, and monitored using modern DevOps tools.
