# All-about-jenkins
## How to install jenkins on ubuntu
#### Step 1:- create ec2 instance  with instncetype min (RAM 4GB) and allow port no 8080
#### Step 2 :- first we have to install Jdk (java)
              
              java --version # to check java version or to check jdk is installed or not
              apt update 
              apt install openjdk-21-jre
              
#### Step 3:- Now we are installing jenkins in ubuntu
            
            sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
              https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
            echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
              https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
              /etc/apt/sources.list.d/jenkins.list > /dev/null
            sudo apt update
            sudo apt install jenkins
          
#### Step 4: hit the public ip of your instance on google with port 8080 ex (xx.xx.xx.xx:8080)
#### Step 5:- get the password of jenkins and login
            
            vim /var/lib/jenkins/secrets/initialAdminPassword

Perfect 👍
Below is a **GitHub-ready `README.md`** you can **directly copy-paste** into your repository.
It is **clean, structured, CLI-only, and interview-safe**.

---

# 🚀 SonarQube Installation on Ubuntu (CLI)

This repository provides **step-by-step instructions** to install **SonarQube** on **Ubuntu** using **command line only**.

---

## 🧱 Architecture Overview

![Image](https://www.oreilly.com/api/v2/epubs/9781838642730/files/assets/296094c9-8e76-4fc4-88a6-3a16060f10bc.png)

![Image](https://scm.thm.de/sonar/images/embed-doc/images/SQ-instance-components.png)

SonarQube uses:

* Java 17
* PostgreSQL (database)
* Elasticsearch (bundled)
* Web UI on port **9000**

---

## ✅ Prerequisites

* Ubuntu **20.04 / 22.04**
* Minimum **2 GB RAM**
* `sudo` privileges

---

## 📌 Step 1: Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 📌 Step 2: Install Java 17

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

✔ Ensure Java **17** is installed.

---

## 📌 Step 3: Install PostgreSQL

```bash
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

## 📌 Step 4: Create Database & User

### Login as postgres

```bash
sudo -i -u postgres
```

### Open PostgreSQL shell

```bash
psql
```

### Run SQL commands (INSIDE psql)

```sql
CREATE DATABASE sonarqube;
CREATE USER sonar WITH PASSWORD 'sonar';

ALTER DATABASE sonarqube OWNER TO sonar;
GRANT ALL ON SCHEMA public TO sonar;
ALTER SCHEMA public OWNER TO sonar;

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO sonar;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO sonar;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public TO sonar;
```

Exit:

```sql
\q
exit
```

---

## 📌 Step 5: Configure Kernel Limits (MANDATORY)

```bash
sudo nano /etc/sysctl.conf
```

Add:

```conf
vm.max_map_count=262144
fs.file-max=65536
```

Apply:

```bash
sudo sysctl -p
```

---

## 📌 Step 6: Download SonarQube

```bash
cd /opt
sudo apt install unzip -y
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.2.77730.zip
sudo unzip sonarqube-9.9.2.77730.zip
sudo mv sonarqube-9.9.2.77730 sonarqube
```

---

## 📌 Step 7: Create SonarQube User

```bash
sudo useradd sonar
sudo chown -R sonar:sonar /opt/sonarqube
```

---

## 📌 Step 8: Configure Database Connection

```bash
sudo nano /opt/sonarqube/conf/sonar.properties
```

Update:

```properties
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
```

---

## 📌 Step 9: Create systemd Service

```bash
sudo nano /etc/systemd/system/sonarqube.service
```

```ini
[Unit]
Description=SonarQube service
After=network.target postgresql.service

[Service]
Type=forking
User=sonar
Group=sonar
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
LimitNOFILE=65536
LimitNPROC=4096
Restart=always

[Install]
WantedBy=multi-user.target
```

---

## 📌 Step 10: Start SonarQube

```bash
sudo systemctl daemon-reload
sudo systemctl start sonarqube
sudo systemctl enable sonarqube
```

Check status:

```bash
sudo systemctl status sonarqube
```

---

## 🌐 Access SonarQube Web UI

```
http://<SERVER-IP>:9000
```

**Default Credentials**

```
Username: admin
Password: admin
```

---

## 🔍 Verification Commands

```bash
sudo ss -tulnp | grep 9000
sudo tail -n 20 /opt/sonarqube/logs/web.log
```

---

## ❗ Common Issues

| Issue              | Cause                        |
| ------------------ | ---------------------------- |
| GUI not loading    | PostgreSQL permission issue  |
| Service restarting | `vm.max_map_count` not set   |
| DB error           | Schema ownership not correct |

---

## 🎯 Interview-Ready Summary

> SonarQube is installed using Java 17 and PostgreSQL as backend.
> Proper kernel tuning and database schema permissions are mandatory.
> It runs as a systemd service and is accessed via port 9000.
