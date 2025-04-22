# MongoDB Setup on VPS (English Documentation)

## Install and Start MongoDB

```bash
sudo apt-get install gnupg curl
```

```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
```

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install -y mongodb-org
```

## Start and Enable MongoDB

```bash
sudo systemctl daemon-reload
sudo systemctl start mongod
sudo systemctl enable mongod
sudo systemctl status mongod
```

## Configure Firewall

```bash
sudo ufw enable
sudo ufw allow 27017
sudo ufw allow 'OpenSSH'
```

## Create Super User

```bash
mongosh
```
```js
use admin
db.createUser({user: "your_admin_username",pwd: passwordPrompt(),roles: ["root"]})
.exit
```

### Enable Security in mongod.conf

```bash
sudo nano /etc/mongod.conf
```

Add the following lines at the bottom:

```yaml
security:
  authorization: enabled

net:
  port: 27017
  bindIp: 0.0.0.0 
```

```bash
sudo service mongod restart
```

## Create Database and User for Project

```bash
mongosh --port 27017 --authenticationDatabase "admin" -u "your_admin_username" -p "your_admin_password"
```

```js
use your_project_db
db.createUser({user: "your_project_user",pwd: passwordPrompt(),roles: [ { role: "readWrite", db: "your_project_db" } ]})
.exit
```

## MongoDB URI to Connect to Project

```
mongodb://your_project_user:your_password@127.0.0.1:27017/your_project_db
```



## ✅ Step-by-Step: Open Port 27017 on AWS EC2

### 🔸 Step 1: Go to AWS EC2 Dashboard
- Open your AWS Console
- Navigate to **EC2 > Instances**
- Click on your instance (the VPS where MongoDB is installed)

### 🔸 Step 2: Find the Security Group
- In the **bottom panel**, find the **Security Group** linked to your instance
- Click on the **Security Group ID** (e.g., `sg-xxxxxxxxxxxxxxx`)

### 🔸 Step 3: Edit Inbound Rules
- On the Security Group page, go to the **Inbound Rules** tab
- Click the **Edit inbound rules** button

### 🔸 Step 4: Add New Rule for MongoDB
In the **Add Rule** section, add the following:

| Field        | Value                          |
|--------------|--------------------------------|
| **Type**     | Custom TCP                     |
| **Protocol** | TCP                            |
| **Port Range** | 27017                        |
| **Source**   | `0.0.0.0/0` (for public testing)<br>or `your IP/32` for security |

🔐 **Security Tip**:  
Use `0.0.0.0/0` **only for development or testing**, not in production.  
For production, use your current IP — for example, if your IP is `123.45.67.89`, then use:

```
123.45.67.89/32
```

This means **only you** can access port `27017`.


---

This way your MongoDB is properly installed and configured on your VPS for production use.

Let me know if you need help with any of the steps!