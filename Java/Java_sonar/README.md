# 🚀 Steps to Start an EC2 Instance, Install Java 11, and Deploy a Java Application

This guide outlines the steps performed on an AWS EC2 instance to install Java 11, clone a Git repository, build a Maven project, install and configure Tomcat, and deploy a WAR file.

---

## 1. Connect to EC2 Instance via SSH

> *(Steps to navigate the AWS console to the EC2 dashboard are assumed to be done through the web interface.)*

- Open your local terminal.
- Navigate to the directory containing your SSH private key file:

```bash
cd Downloads/
```
- Establish an SSH connection to your EC2 instance using the provided key and public DNS:

```bash
ssh -i "Test.pem" ubuntu@ec2-98-81-227-245.compute-1.amazonaws.com
```

---

## 2. Install Java 11

- Update the package lists to ensure you have the latest versions:

```bash
sudo apt update
```

- Install the headless version of OpenJDK 11:

```bash
sudo apt install openjdk-11-jre-headless

## 3. Build the Project with Maven

- Install Maven:

```bash
sudo apt install maven
```

- Navigate to the cloned project directory:

```bash
cd Train ticket reservation system 
```

- Validate the Maven project configuration:

```bash
mvn validate
```

- Run the project tests:

```bash
mvn test
```

- Package the project (assuming it generates a WAR file):

```bash
mvn package
```

> *Note: The output WAR file will typically be in the `target/` subdirectory.*

---

## 5. Install and Configure Apache Tomcat

- To install sonarqube:

```bash
wget  https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip
```

- Extract the downloaded zip file:

```bash
unzip <file>
```

- Navigate to the sonar bin directory:

```bash
cd sonarqube-9.0.102/bin
```

- Start the sonarqube server:

```bash
./startup.sh
```

- To stop the sonarqube server:

```bash
./shutdown.sh
```

---

## 6. Configure Security Group for Tomcat Access

> *(Steps to navigate to the EC2 Security Group through the AWS console are assumed.)*

- Edit the **Inbound Rules** of the security group.
- Add a rule:
  - **Type:** All TCP
  - **Source:** Anywhere IPv4 (`0.0.0.0/0`)

⚠️ *Warning: Allowing traffic from anywhere on all TCP ports is a security risk. For production, restrict access to specific ports and IP ranges.*

---
Hence we finally test the java code

---

📌 **Note:** This guide assumes a basic setup. For production environments, consider security hardening, firewall rules, HTTPS setup, and scalability measures.
