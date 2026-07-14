# aws-linux-webserver

# AWS Linux Web Server Deployment with Docker

An automated and containerized deployment of a custom static portfolio website hosted on AWS (EC2) using Nginx and Docker. This project demonstrates foundational cloud infrastructure, Linux system administration, and containerization skills.

---

## 🏗️ Architecture Diagram

```text
                        ┌────────────────────────┐
                        │        Internet        │
                        └───────────┬────────────┘
                                    │ HTTP (Port 80)
                                    ▼
                        ┌────────────────────────┐
                        │  AWS Security Group   │ (Allows SSH & HTTP)
                        └───────────┬────────────┘
                                    │
                                    ▼
                     ┌──────────────────────────────┐
                     │     AWS EC2 (Ubuntu VM)      │
                     │  ┌────────────────────────┐  │
                     │  │     Docker Daemon      │  │
                     │  │  ┌──────────────────┐  │  │
                     │  │  │ Nginx Container  │  │  │
                     │  │  │ (Port 80 -> 80)  │  │  │
                     │  │  │        ▲         │  │  │
                     │  │  └────────┼─────────┘  │  │
                     │  │           │ Read-Only  │  │
                     │  │           ▼ Volume     │  │
                     │  │   [index.html on Host] │  │
                     │  └────────────────────────┘  │
                     └──────────────────────────────┘

🛠️ Tech Stack & Skills Demonstrated
Cloud Platform: Amazon Web Services (AWS EC2, VPC, Security Groups)

Containerization: Docker (Engine, Volume Bind Mounts, Lifecycle Management)

Web Server: Nginx (Static Content Delivery)

Operating System: Linux (Ubuntu Server administration, SSH, Permissions/Chmod)

Documentation: Markdown, Architecture-as-Code

🚀 Step-by-Step Deployment Guide
1. Infrastructure Provisioning
Launched an EC2 Instance on AWS utilizing the Ubuntu Server 24.04 LTS AMI (t3.micro ).

Configured an AWS Security Group to restrict inbound traffic:

Port 22 (SSH): Restricted to administrator IP for secure shell management.

Port 80 (HTTP): Opened to the public (0.0.0.0/0) to serve the web application.


2. Docker Installation & Configuration
Connected to the instance via SSH and initialized Docker with the following system admin commands:


# Update local package manager index
sudo apt update && sudo apt upgrade -y

# Install Docker engine
sudo apt install docker.io -y

# Enable Docker to automatically start on boot
sudo systemctl enable docker
sudo systemctl start docker

# Add the ubuntu user to the docker group to run commands without sudo
sudo usermod -aG docker ubuntu

3. Custom Website Setup & Volume Mapping
To ensure the website code remains independent of the container lifecycle, a custom local workspace was created on the host and bind-mounted to the container:

# Set up host workspace directory
mkdir -p /home/ubuntu/mywebsite
cd /home/ubuntu/mywebsite

# Created a custom portfolio landing page (index.html)
# Configured required read permissions for the non-root container process:
chmod 755 /home/ubuntu/mywebsite
chmod 644 /home/ubuntu/mywebsite/index.html


4. Running the Container
Executed the container in detached background mode, pointing Nginx's document root to our host workspace:
docker run -d \
  --name nginx \
  -p 80:80 \
  -v $(pwd):/usr/share/nginx/html:ro \
  nginx


🔍 Verification & Logs
Check Running Containers and Verify Downloaded Images

docker ps
docker images








Check Web Server Output

docker exec -it nginx cat /usr/share/nginx/html/index.html


📸 Final Live Deployment
The application is successfully running and fully accessible at:
👉 http://51.21.170.145
'''
<img width="1890" height="894" alt="104dd8b8-0735-4a04-b3b7-cf4eb16824dc" src="https://github.com/user-attachments/assets/0f4efea9-01ca-4f89-ae05-6774aec8f942" />

