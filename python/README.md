
# Deploy Python Flask App with Gunicorn on Amazon Linux

## Prerequisites

- Amazon Linux EC2 instance running
- Root/sudo access
- Python 3 and pip
- Git installed
- Flask app files with `requirements.txt`

---

## 1. Switch to Root and Update the System

```bash
sudo -i
yum update -y
```

---

## 2. Check and Install Python 3

Check if Python 3 is installed:

```bash
yum list installed | grep -i python3
```

If not installed:

```bash
sudo yum install python3 -y
```

---

## 3. Install Required Packages

```bash
yum install git -y
yum install python3-pip -y
```

---

## 4. Clone or Navigate to Flask App Directory

```bash
git clone <git-url>
cd /root/example-voting-app/vote
```

---

## 5. Install Python Dependencies

```bash
pip3 install -r requirements.txt
pip3 install flask
pip3 install redis
pip3 install gunicorn
```

---

## 6. Create and Activate Virtual Environment

```bash
python3 -m venv myenv
source myenv/bin/activate
```

Your shell prompt should change, indicating you're in the virtual environment.

---

## 7. Run Flask App (Test in Dev Mode)

```bash
python3 app.py
```

Expected output:

```
 * Serving Flask app 'app'
 * Debug mode: on
 * Running on http://127.0.0.1:80
 * Running on http://<PRIVATE_IP>:80
```

---

## 8. Install and Test Gunicorn

```bash
pip3 install gunicorn
gunicorn app:app -b 0.0.0.0:80 --log-file - --access-logfile - --workers 4 --keep-alive 0
```

---

## 9. Create systemd Service for Flask App

```bash
vi /etc/systemd/system/vote.service
```

Add the following:

```ini
[Unit]
Description=Gunicorn instance to serve Flask app
After=network.target

[Service]
User=root
Group=root
WorkingDirectory=/root/example-voting-app/vote
ExecStart=/root/example-voting-app/vote/myenv/bin/gunicorn app:app -b 0.0.0.0:80 --log-file - --access-logfile - --workers 4 --keep-alive 0
TimeoutStopSec=90
Restart=always
KillMode=mixed

[Install]
WantedBy=multi-user.target
```

---

## 10. Start and Enable the Flask Service

```bash
systemctl daemon-reload
systemctl start vote.service
systemctl status vote.service
systemctl enable vote.service
```

---

## 11. Access the Application

Visit: `http://<EC2_PUBLIC_IP>:80`

---

## Example Command History

```bash
yum update -y
yum install git -y
git clone https://github.com/Ai-TechNov/example-voting-app
cd example-voting-app/vote/
yum install pip3 -y
yum install python3-pip -y
pip3 install -r requirements.txt
python3 -m venv myenv
source myenv/bin/activate
pip3 install flask redis gunicorn
python3 app.py
gunicorn app:app -b 0.0.0.0:80 --log-file - --access-logfile - --workers 4 --keep-alive 0
vi /etc/systemd/system/vote.service
systemctl daemon-reload
systemctl start vote.service
systemctl status vote.service
systemctl enable vote.service
```