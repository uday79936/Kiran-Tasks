
# Deploying Flask App with Gunicorn on AWS EC2

## Connect to the EC2 Instance

Use the SSH key to connect to your server:

```bash
ssh -i "python1.pem" ubuntu@ec2-98-84-171-254.compute-1.amazonaws.com
```

---

## 1. Create a Project Folder 

```bash
mkdir app1
cd app1
```

---

## 2. Create a Virtual Environment

```bash
python3 -m venv myenv
source myenv/bin/activate
```

---

## 3. Install Flask and Gunicorn

```bash
pip install flask gunicorn
```

---

## 4. Create Your Flask App

Create a file named `app.py`:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask and Gunicorn!"
```

---

## 5. Run Your App with Gunicorn

Run this command to start your Flask app with Gunicorn:

```bash
gunicorn --bind=0.0.0.0:5001 app:app
```

- `app` (before the colon) is the **filename** (i.e., `app.py` without the `.py`)
- `app` (after the colon) is the **Flask app variable** you defined

> ⚠️ Make sure you're in the same directory as `app.py` when you run the command.

---

## 6. Access Your App in a Browser

Ensure **port 5001** is open in your AWS EC2 Security Group.

Then open your app in a browser:

```
http://<your-ec2-public-ip>:5001
```

Example:

```
http://ec2-98-84-171-254.compute-1.amazonaws.com:5001
```

---

## 7. (Optional) Use `wsgi.py` for Better Structure

Create a file named `wsgi.py`:

```python
# wsgi.py
from app import app

if __name__ == "__main__":
    app.run()
```

Then run:

```bash
gunicorn --bind=0.0.0.0:5001 wsgi:app



##Deploy output



