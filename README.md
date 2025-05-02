# 🚀 Python Flask Deployment Guide

This repository is used for deploying a **Python Flask application** on a Linux server using **Nginx** as a reverse proxy.

---

## 🧰 Tools & Technologies Used

<table>
  <tr>
    <td><img src="https://img.shields.io/badge/Amazon%20EC2-%23232F3E.svg?style=for-the-badge&logo=amazon-ec2&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Amazon%20Linux-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white" /></td>
  </tr>
  <tr>
    <td><img src="https://img.shields.io/badge/pip-3775A9?style=for-the-badge&logo=pypi&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white" /></td>
    <td><img src="https://img.shields.io/badge/Bash-121011?style=for-the-badge&logo=gnu-bash&logoColor=white" /></td>
  </tr>
</table>

---

## 📦 Prerequisites

- A Linux server (e.g., Amazon EC2)
- Port 80 (HTTP) and port 5000 (Flask default) opened in the security group
- Git installed on the server
- Nginx installed

---

## 🛠️ Step-by-Step Setup

### 1️⃣ Clone the Repository

SSH into your server and install **Git**:

```bash
sudo yum install git -y
```

Clone the repository:

```bash
git clone https://github.com/yourusername/your-repo-name.git
cd your-repo-name
```

---

### 2️⃣ Install Python and pip

Install Python 3 and pip:

```bash
sudo yum install python3-pip -y
```

---

### 3️⃣ Install Flask and Other Dependencies

Create a `requirements.txt` file:

```txt
flask
```

Install all dependencies:

```bash
pip3 install -r requirements.txt
```

---

### 4️⃣ Run the Flask App

Start the Flask app:

```bash
python3 app.py
```

It will run on:

```
http://<your-server-ip>:5000
```

---

### ⚠️ Flask App Stops on Logout?

To keep the app running in the background:

```bash
nohup python3 app.py &
```

To store logs in a file:

```bash
nohup python3 app.py > output.log 2>&1 &
```

---

### 🔍 Monitor or Stop the App

Check if it's running:

```bash
ps aux | grep app.py
```

Stop the app:

```bash
pkill -f app.py
```

---

## 🌐 Run on Port 80 with Nginx

### 1️⃣ Install Nginx

```bash
sudo yum install nginx -y
```

### 2️⃣ Configure Nginx

Edit the config file:

```bash
sudo vi /etc/nginx/conf.d/flask-app.conf
```

Paste this:

```nginx
server {
    listen 80;
    server_name 34.228.170.42;

    location / {
        proxy_pass http://34.228.170.42:5000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 3️⃣ Start and Enable Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Now you can visit:

```
http://34.228.170.42
```

Without the port number!

---

## ✅ Summary

- Clone repo → Install Python & pip → Install Flask via `requirements.txt`
- Use `nohup` to run Flask persistently
- Configure Nginx to expose app on port 80

---

## 🧼 Optional Maintenance

Check running processes:

```bash
ps aux | grep app.py
```

Kill app:

```bash
pkill -f app.py
```

View logs:

```bash
cat output.log
```

---

## 📬 Need Help?

Open an issue in this repo or reach out!
