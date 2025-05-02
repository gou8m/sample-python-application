# 🚀 Python Flask Deployment Guide

This repository is used for deploying a **Python Flask application** on a Linux server using **Nginx** as a reverse proxy.

---

## 📦 Prerequisites

- A Linux server (e.g., Amazon EC2)
- Port 80 (HTTP) and port 5000 (Flask default) opened in the security group
- Git installed on the server
- Nginx installed

---

## 🛠️ Step-by-Step Setup

### 1️⃣ Clone the Repository

First, SSH into your server and install **Git**:

```bash
sudo yum install git -y
```

Then clone the repository:

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

Instead of installing packages one by one, we list them in a `requirements.txt` file.

**Create a file named `requirements.txt`** with the following:

```txt
flask
```

Install all dependencies with:

```bash
pip3 install -r requirements.txt
```

---

### 4️⃣ Run the Flask App

Run the Flask app (assumes your main file is `app.py`):

```bash
python3 app.py
```

Now the app will be running on:

```
http://<your-server-ip>:5000
```

---

### ⚠️ Flask App Stops on Logout?

By default, Flask runs in **interactive mode**, so the process stops when you log out.

To run it in the background and keep it alive:

```bash
nohup python3 app.py &
```

To store logs in a file:

```bash
nohup python3 app.py > output.log 2>&1 &
```

### 🔍 Check if it's Running:

```bash
ps aux | grep app.py
```

### ❌ To Stop the App:

```bash
pkill -f app.py
```

---

## 🌐 Set Up Nginx as a Reverse Proxy (Run App on Port 80)

### 1️⃣ Install Nginx

```bash
sudo yum install nginx -y
```

### 2️⃣ Configure Nginx

Edit or create the file:

```bash
sudo vi /etc/nginx/conf.d/flask-app.conf
```

Paste the following configuration:

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

Now, you can **access the app without the port**:

```
http://34.228.170.42
```

---

## ✅ Summary

- Clone repo → Install Python/pip → Install Flask via `requirements.txt`
- Run app with `nohup` to keep it alive after logout
- Use Nginx to run it on port 80 instead of 5000

---

## 🧼 Optional: Clean Process & Logs

To check the app status:

```bash
ps aux | grep app.py
```

To kill the running app:

```bash
pkill -f app.py
```

Log file created by `nohup`:

```bash
cat output.log
```

---

## 📬 Need Help?

Open an issue in this repo or reach out with questions!

