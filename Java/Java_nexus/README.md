# Connecting Two EC2 Instances and Deploying Java Project to Nexus

## 1. Connect to the Java Build Server

```bash
ssh -i "Java_build.pem" ubuntu@ec2-54-145-114-194.compute-1.amazonaws.com
```

### Update and Install Dependencies

```bash
sudo apt update
sudo apt install git
git --version
```

### Clone the Repository

```bash
git clone https://github.com/shashirajraja/Train-Ticket-Reservation-System.git
```

### Install Java Runtime

```bash
sudo apt install openjdk-11-jre-headless
```

### Build the Project

```bash
mvn test
mvn package
```

> A `target/` folder will be generated containing the `.war` file.

---

## 2. Nexus Server Setup

### Update and Install Java

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

### Download and Extract Nexus

```bash
wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.79.0-09.tar.gz
tar -xvf nexus-unix-x86-64-3.79.0-09.tar.gz
sudo mv nexus-3* /opt/nexus
sudo mv sonatype /opt/
```

### Create Nexus User and Set Permissions

```bash
sudo adduser nexus
sudo chown -R nexus:nexus /opt/nexus
sudo chown -R nexus:nexus /opt/sonatype-work
```

### Create a Systemd Service for Nexus

```bash
sudo nano /etc/systemd/system/nexus.service
```

Paste the following configuration:

```ini
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
User=nexus
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

### Enable and Start Nexus Service

```bash
sudo systemctl enable nexus
sudo systemctl start nexus
```

> Access Nexus at: `http://<your-server-ip>:8081`  
> Retrieve the admin password:

```bash
cat /nexus-data/admin.password
```

---

## 3. Deploy from Java Build Server to Nexus

### Update `pom.xml`

Edit the `pom.xml` file:

```bash
vim pom.xml
```

Insert the following after `</dependencies>`:

```xml
<distributionManagement>
  <repository>
    <id>maven-releases</id>
    <url>http://172.178.116.75:8081/repository/maven-releases/</url>
  </repository>
</distributionManagement>
```

### Configure Maven Credentials

```bash
vim /etc/maven/settings.xml
```

Add the following inside the `<servers>` block:

```xml
<server>
  <id>maven-releases</id>
  <username>admin</username>
  <password>admin123</password>
</server>
```

### Clean and Deploy the Project

```bash
mvn clean deploy -X
```
![Image](https://github.com/user-attachments/assets/e15dd2eb-07a2-4ff1-96d5-02d3855a2b23)

> Maven will build the artifact and deploy it to the Nexus releases repository.
