# 🧪 Docker Lab: Flask App on AWS EC2 (Ubuntu Edition)

## 📌 Objective
Learn to deploy a Flask web application inside a Docker container on an Ubuntu EC2 instance using AWS.

---

## ✅ Step 1: Launch EC2 Instance

1. **Launch a new EC2 instance**
   - **OS:** Ubuntu 22.04
   - **Type:** `t2.micro` (Free Tier eligible)
   - **Key Pair:** Use or create `.pem` key
   - **Security Group Inbound Rules:**
     - **SSH (22)** – From your IP
     - **HTTP (80)** – From Anywhere

2. **Connect to your EC2 instance**
   ```bash
   ssh -i your-key.pem ubuntu@your-ec2-public-ip
   ```
## Step 2: Install Docker & Docker Compose
```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install Docker and Docker Compose
sudo apt install -y docker.io docker-compose

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to the Docker group
sudo usermod -aG docker ubuntu

# Apply group change (without reboot)
newgrp docker
```
## Step 3: Create the Flask App
```cpp
flask-docker-lab/
├── app.py
├── static/
│   └── tech.gif
└── templates/
    └── index.html
```

```bash
mkdir flask-docker-lab && cd flask-docker-lab
```

Create app.py
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```

create templates/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome to Flask on Docker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding-top: 50px;
            background-color: #f4f4f4;
        }
        h1 {
            color: #333;
        }
        img {
            width: 400px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>🚀 Hello from Flask on Ubuntu in Docker!</h1>
    <p>This is running inside a Docker container on your EC2 instance.</p>
    <img src="https://media.giphy.com/media/3o6ZtaO9BZHcOjmErm/giphy.gif](https://giphy.com/gifs/computer-computing-retro-fC4eVInYkUAR0a5hpK" alt="Tech GIF">
</body>
</html>
```

## Step 4: Write a Dockerfile
```docker
FROM python:3.9-slim

# Create a non-root user and group
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser

# Set working directory
WORKDIR /app

# Copy all application files (including templates and static folders)
COPY . .

# Install dependencies
RUN pip install flask

# Change ownership to non-root user
RUN chown -R appuser:appgroup /app

# Switch to non-root user
USER appuser

# Expose the Flask port
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```
## Step 5: Build & Run the Docker Container
```bash
docker build -t flask-app.
docker run -d -p 80:5000 flask-app
```
## Step 6: Test in your browser
Open a browser and visit:
```arduino
http://your-ec2-public-ip
```
You should see:
```csharp
Hello from Flask on Ubuntu in Docker!
```

