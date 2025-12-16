# All-about-jenkins
## How to install jenkins on ubuntu
#### Step 1:- create ec2 instance  with instncetype min (RAM 4GB) and allow port no 8080
#### Step 2 :- first we have to install Jdk (java)
              ```bash
              java --version # to check java version or to check jdk is installed or not
              apt update 
              apt install openjdk-21-jre
              ```
#### Step 3:- Now we are installing jenkins in ubuntu
            ```bash
            sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
              https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
            echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
              https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
              /etc/apt/sources.list.d/jenkins.list > /dev/null
            sudo apt update
            sudo apt install jenkins
            ```
#### Step 4: hit the public ip of your instance on google with port 8080 ex (xx.xx.xx.xx:8080)
#### Step 5:- get the password of jenkins and login
            ```bash
            vim /var/lib/jenkins/secrets/initialAdminPassword

