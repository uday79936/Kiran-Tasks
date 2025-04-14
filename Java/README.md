Steps to Start an EC2 Instance, Install Java 11, and Deploy a Java Application
This guide outlines the steps performed on an AWS EC2 instance to install Java 11, clone a Git repository, build a Maven project, install and configure Tomcat, and deploy a WAR file.

1. Navigate AWS Console and Connect to EC2 Instance via SSH:

(Steps to navigate the AWS console to the EC2 dashboard are assumed to be done through the web interface.)
Open your local terminal.
Navigate to the directory containing your SSH private key file:
cd Downloads/
Establish an SSH connection to your EC2 instance using the provided key and public DNS:
ssh -i "Test.pem" ubuntu@ec2-98-81-227-245.compute-1.amazonaws.com
2. Install Java 11:

Update the package lists to ensure you have the latest versions:
sudo apt update
Install the headless version of OpenJDK 11:
sudo apt install openjdk-11-jre-headless
3. Clone the Project from Git:

Navigate to the home directory (if not already there):
cd ~
Clone the specified Git repository:
git clone https://github.com/shashirajraja/Train-Ticket-Reservation-System.git
4. Build the Project with Maven:

Install Maven:
sudo apt install maven
Navigate to the cloned project directory:
cd Calculator
Validate the Maven project configuration:
mvn validate
Run the project tests:
mvn test
Package the project (assuming it generates a WAR file):
mvn package
(Note: The output WAR file will typically be in the target subdirectory.)
5. Install and Configure Apache Tomcat:

Download the Apache Tomcat 9 tar.gz file using wget:
wget [https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.102/bin/apache-tomcat-9.0.102.tar.gz](https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.102/bin/apache-tomcat-9.0.102.tar.gz)
Extract the downloaded tar.gz file:
tar -xvf apache-tomcat-9.0.102.tar.gz
Navigate to the Tomcat bin directory:
cd apache-tomcat-9.0.102/bin
Start the Tomcat server:
./startup.sh
To stop the Tomcat server:
./shutdown.sh
6. Configure Security Group for Tomcat Access:

(Steps to navigate to the EC2 Security Group associated with your instance through the AWS console are assumed.)
Edit the inbound rules of the security group.
Add a new rule allowing traffic on All TCP ports from Anywhere IPv4 (0.0.0.0/0).
Warning: Allowing traffic from anywhere on all TCP ports is a security risk. For production environments, restrict access to specific ports and IP ranges as needed.
7. Deploy the WAR File:

Copy the generated WAR file (assuming it's named TrainBook-1.0.0-SNAPSHOT.war and located in the target directory of your cloned project) to the Tomcat webapps directory. Adjust the paths accordingly if your WAR file has a different name or location:
cp ~/Calculator/target/TrainBook-1.0.0-SNAPSHOT.war ~/apache-tomcat-9.0.102/webapps/
(Note: The path ~/Train-Ticket-Reservation-System/apache-tomcat-9.0.102/webapps in the original request seems incorrect based on the previous steps. Assuming the user intended to deploy to the standard Tomcat webapps directory.)
8. (Optional) Configure Tomcat Manager and Host Manager:

Navigate to the manager web application directory within Tomcat:

cd ~/apache-tomcat-9.0.102/webapps/manager/
(The request mentions commenting out a <Valve> element, but the specific file and purpose are unclear without more context. This step might involve editing the context.xml file within the manager web application's META-INF directory.)

Navigate to the host-manager web application directory within Tomcat:

cd ~/apache-tomcat-9.0.102/webapps/host-manager/
(Similar to the manager app, commenting out a <Valve> element might involve editing the context.xml file within the host-manager web application's META-INF directory.)

9. (Optional) Configure Tomcat Users:

Navigate to the Tomcat conf directory:
cd ~/apache-tomcat-9.0.102/conf/
Edit the tomcat-users.xml file to add or modify user credentials for accessing the Tomcat Manager and Host Manager applications.
10. Verify Deployment:

Restart the Tomcat server:
~/apache-tomcat-9.0.102/bin/shutdown.sh
~/apache-tomcat-9.0.102/bin/startup.sh
Open your web browser and access your application using the EC2 instance's public IP address or public DNS name, followed by the context path of your application (which is usually the WAR file name without the .war extension). For example: http://ec2-98-81-227-245.compute-1.amazonaws.com:8080/TrainBook-1.0.0-SNAPSHOT/ (assuming Tomcat is running on the default port 8080).
Note: This guide assumes a basic setup. Production environments often require more advanced configurations for security, performance, and scalability. Remember to replace placeholder values (like IP addresses and file names) with your actual values.