BloodHound Community Edition (CE) Setup Guide on Kali Linux (VM)

BloodHound CE is a powerful tool for analyzing Active Directory (AD) environments and visualizing relationships that could lead to privilege escalation.

This guide walks you through the installation and setup of BloodHound CE using Docker Compose on Kali Linux (running as a Virtual Machine).

## 🔧 Prerequisites

✅ Kali Linux (up-to-date) running in a Virtual Machine

✅ Internet access

✅ A user account with sudo privileges

## 🚀 Step-by-Step Installation:

### 🐳 Step 1: Install Docker

- **Docker allows BloodHound CE to run as containerized services.**

```
sudo apt update

sudo apt install docker.io -y
```


**Verify Docker is installed:**

```
docker --version
```

### 📦 Step 2: Install Docker Compose

**Docker Compose orchestrates multiple container services.**


```
sudo apt install docker-compose -y
```

**Check version:**

```
docker-compose --version
```


### 📥 Step 3: Download BloodHound CE Docker Compose File

**Retrieve the pre-configured Docker Compose file:**

```
curl -L [https://ghst.ly/getbhce](https://ghst.ly/getbhce) -o docker-compose.yml
```

🔗 For official documentation:

[BloodHound CE Install Docs](https://bloodhound.specterops.io/get-started/quickstart/community-edition-quickstart)

### 🔄 Step 4: Pull & Run BloodHound CE Containers

**This step downloads the images and starts the BloodHound CE services.**

```
docker-compose pull && docker-compose up -d
```

`pull`: Downloads required images

`up -d`: Starts services in detached mode (background)

### 👤 Step 5: Add Current User to Docker Group

**To run Docker without sudo every time:**

```
sudo usermod -aG docker $USER

newgrp docker
```


💡 You may need to log out and back in if **newgrp** doesn’t work immediately.

### 📋 Step 6: Verify Running Containers

**Check if BloodHound containers are active:**

```
docker ps
```

Look for containers named something like bloodhound... and ensure they're listed as "Up".

### ▶️ Step 7: Start (or Restart) BloodHound CE (if needed)

**In case you need to start or restart manually:**

```
docker-compose -f docker-compose.yml up -d
```

📜 If you don’t see a login password:

```
sudo docker-compose -f docker-compose.yml logs
```

**Look for a line like:**

"Initial Password Set To: [some-random-password]"

### 🌐 Step 8: Access the BloodHound CE Web Interface

**Open your browser and go to:**

[http://localhost:8080/ui/login](http://localhost:8080/ui/login)

Login credentials:

`Username: admin`

`Password: (the auto-generated password from logs)`

🔐 You will be prompted to set a new password after first login.

### 📊 Step 9: Welcome to the BloodHound Dashboard

You're in! You should now see the BloodHound CE dashboard.

You can now:

1. Create or import datasets

2. Visualize Active Directory attack paths

3. Explore built-in queries and analytics

## 🛠️ Troubleshooting Tips:

### 🔄 Containers not running?

Run to see status:

```
docker-compose ps 
```

or check logs with:

```
docker-compose logs -f
```

### 🔐 Forgot password?

Re-run docker-compose logs to see if the password is shown again. Otherwise, delete volumes and redeploy.

### 🌐 Web interface not loading?

Ensure port 8080 is not blocked or used by another service.


**Next step:**  [[Bloodhound analysis]]
