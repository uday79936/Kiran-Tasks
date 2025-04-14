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
```

---

## 3. Clone the Project from Git

- Navigate to the home directory (if not already there):

```bash
cd ~
```

- Clone the specified Git repository:

```bash
git clone https://github.com/shashirajraja/Train-Ticket-Reservation-System.git
```

---

## 4. Build the Project with Maven

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

- Download Apache Tomcat 9:

```bash
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.102/bin/apache-tomcat-9.0.102.tar.gz
```

- Extract the downloaded tar.gz file:

```bash
tar -xvf apache-tomcat-9.0.102.tar.gz
```

- Navigate to the Tomcat bin directory:

```bash
cd apache-tomcat-9.0.102/bin
```

- Start the Tomcat server:

```bash
./startup.sh
```

- To stop the Tomcat server:

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

## 7. Deploy the WAR File

- Copy the WAR file to the Tomcat webapps directory (adjust paths as needed):

```bash
cp ~/Calculator/target/TrainBook-1.0.0-SNAPSHOT.war ~/apache-tomcat-9.0.102/webapps/
```

> *Note: Adjust file names and paths if different in your project.*

---

## 8. (Optional) Configure Tomcat Manager and Host Manager

- Navigate to the manager web application:

```bash
cd ~/apache-tomcat-9.0.102/webapps/manager/
```

> *Comment out the `<Valve>` element in `META-INF/context.xml` if needed for access.*

- Navigate to the host-manager web application:

```bash
cd ~/apache-tomcat-9.0.102/webapps/host-manager/
```

> *Similarly, edit `META-INF/context.xml` and comment out the `<Valve>` tag.*

---

## 9. (Optional) Configure Tomcat Users

- Navigate to the Tomcat `conf` directory:

```bash
cd ~/apache-tomcat-9.0.102/conf/
```

- Edit `tomcat-users.xml` to add admin access:

```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="admin" password="your_password" roles="manager-gui,admin-gui"/>
</tomcat-users>
```

---

## 10. Verify Deployment

- Restart the Tomcat server:

```bash
~/apache-tomcat-9.0.102/bin/shutdown.sh
~/apache-tomcat-9.0.102/bin/startup.sh
```

- Open your web browser and access your application:

```
http://ec2-98-81-227-245.compute-1.amazonaws.com:8080/TrainBook-1.0.0-SNAPSHOT/
```

> *Assuming Tomcat is running on the default port `8080`, and the WAR file is named `TrainBook-1.0.0-SNAPSHOT.war`.*

---

📌 **Note:** This guide assumes a basic setup. For production environments, consider security hardening, firewall rules, HTTPS setup, and scalability measures.
