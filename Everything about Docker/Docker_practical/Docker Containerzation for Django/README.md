# Docker Containerization for Django Applications

This repository demonstrates how to dockerize and deploy a single-page **Python Django Web Application** inside a Docker container. It is part of the "Docker Zero to Hero" learning track, focusing on providing DevOps engineers with the core competencies needed to bundle, package, and execute standard web applications seamlessly.

## 🚀 Key Features & Topics Covered

* **Django Workflow Demystified:** Understanding `settings.py` (project configurations) and `urls.py` (routing framework) from a DevOps perspective.
* **Environment Isolation:** Bundling source code and exact dependencies (`requirements.txt`) to eliminate cross-platform environment issues.
* **CMD vs ENTRYPOINT:** Deep-dive into configuring non-overrideable base executables alongside dynamic runtime arguments.
* **Port Mapping & Host Access:** Properly routing container ports to host instances for browser accessibility.
* **AWS Security Group Mapping:** Setting up custom inbound rules to whitelist specific service ports.

---

## 🛠️ Tech Stack & Infrastructure

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Language** | Python 3 | Base runtime environment |
| **Framework** | Django | Web framework structure |
| **Containerization** | Docker | Packaging tool |
| **Base Image** | Ubuntu | OS layer chosen for the container environment |
| **Infrastructure** | AWS EC2 | Remote virtual server instance for hosting |

---

## 📝 Dockerfile Reference

The logic containerizes the application on top of an Ubuntu base image, provisions Python, resolves packages, and opens operational ports:

```dockerfile
FROM ubuntu

WORKDIR /app

COPY requirements.txt /app/
COPY devops /app/

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

SHELL ["/bin/bash", "-c"]

RUN python3 -m venv venv1 && \
source venv1/bin/activate && \
pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD source venv1/bin/activate && python3 manage.py runserver 0.0.0.0:8000
```

---

## 🏃‍♂️ Step-by-Step Deployment Guide

### 1. Clone the Code Base
Enter your server or machine terminal and pull the project asset workspace down:
```bash
git clone https://github.com
cd Docker-Zero-to-Hero/python-web-app
```

### 2. Build the Docker Image
Execute a structural build process locally to bundle requirements alongside structural code layers:
```bash
docker build -t django-app .
```

### 3. Run the Container (With Port Mapping)
Simply mapping an internal port without binding it to the host breaks browser visibility. Run the container explicitly passing the `-p` parameter:
```bash
docker run -it -p 8000:8000 django-app
```

### 4. Adjust Cloud Provider Inbound Firewall Rules
If your app loads inside an isolated instance (like an **AWS EC2 instance**), make sure the host firewall permits outside requests:
1. Go to your **AWS Console** -> **EC2 Instances**.
2. Choose your running instance and open the **Security** tab.
3. Click your **Security Group** link and choose **Edit Inbound Rules**.
4. Append a rule: **Custom TCP**, Port: `8000`, Source: `0.0.0.0/0` (Anywhere).

You can now reach your platform directly by opening a web browser and loading: `http://<your-ec2-public-ip>:8000/demo`.

