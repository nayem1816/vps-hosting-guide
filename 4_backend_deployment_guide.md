# 🚀 Backend Deployment (Node.js + Express)

## ✅ Step-by-Step Guide

---

### 🔹 Step 1: Clone the Backend Repository

```bash
mkdir -p /var/www
cd /var/www
git clone https://github.com/yourusername/your-repo.git
cd your-repo/backend
```

---

### 🔹 Step 2: Install Dependencies

```bash
npm install
```

---

### 🔹 Step 3: Create and Configure `.env` File

```bash
nano .env
```

> Add your environment variables here, then save and exit using `Ctrl + X ➝ Y ➝ Enter`.

---

### 🔹 Step 4: Install `pm2` and Start the Server

```bash
npm install -g pm2
pm2 start server.js --name project-backend
```

---

### 🔹 Step 5: Enable Server to Run on Startup

```bash
pm2 startup
pm2 save
```

---

### 🔹 Step 6: Enable Firewall and Allow Backend Port

```bash
sudo ufw status
sudo ufw enable
sudo ufw allow 'OpenSSH'
sudo ufw allow 4000
sudo ufw allow 80
sudo ufw allow 443
```

---

### 🔹 Step 7: Configure Nginx Reverse Proxy for Backend

```bash
sudo nano /etc/nginx/sites-available/api.yourdomain.com.conf
```

```nginx
server {
    listen 80;
    server_name api.yourdomain.com;

    location / {
        proxy_pass http://localhost:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

pm2 start npm --name "my-next-app" -- start
```

#### Enable Site & Restart Nginx

```bash
sudo ln -s /etc/nginx/sites-available/api.yourdomain.com.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

# 🎉 Backend Live: `http://api.yourdomain.com`




# 🚀 SSL with Certbot (HTTPS):

```bash
sudo apt install certbot python3-certbot-nginx
```

```bash
sudo certbot --nginx -d api.yourdomain.com
```
```bash
sudo certbot renew --dry-run
```
```bash
sudo certbot renew
```
