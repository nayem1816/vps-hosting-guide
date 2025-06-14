# 🎨 Frontend Deployment (React.js)

## ✅ Step-by-Step Guide

---

### 🔹 Step 1: Navigate to Your React Project and Install Dependencies

```bash
cd /var/www/your-repo/frontend
npm install
```

---

### 🔹 Step 2: Create and Configure `.env` File (If Required)

```bash
nano .env
```

> Add your environment variables, then save and exit using `Ctrl + X ➝ Y ➝ Enter`.

---

### 🔹 Step 3: Create Production Build

```bash
npm run build
```

---

### 🔹 Step 4: Install Nginx (if not already installed)

```bash
sudo apt update
sudo apt install -y nginx
```

---

### 🔹 Step 5: Allow Nginx Through Firewall

```bash
sudo ufw allow 'Nginx Full'
```

---

### 🔹 Step 6: Configure Nginx for React App

```bash
sudo nano /etc/nginx/sites-available/yourdomain1.com.conf
```

```nginx
server {
    listen 80;
    server_name yourdomain1.com www.yourdomain1.com;

    location / {
        root /var/www/your-repo/frontend/dist;
        try_files $uri /index.html;
    }
}
```

---

### 🔹 Step 7: Enable the Nginx Site and Restart Nginx

```bash
sudo ln -s /etc/nginx/sites-available/yourdomain1.com.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

### 🔹 Step 8: Repeat for Additional React Apps (if any)

```bash
sudo nano /etc/nginx/sites-available/yourdomain2.com.conf
```

```nginx
server {
    listen 80;
    server_name yourdomain2.com www.yourdomain2.com;

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/yourdomain2.com.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

# 🎉 Frontend Live:  
- `http://yourdomain1.com`  
- `http://yourdomain2.com` (if applicable)




# 🚀 SSL with Certbot (HTTPS):

```bash
sudo apt install certbot python3-certbot-nginx
```

```bash
sudo certbot --nginx -d yourdomain.com
```
```bash
sudo certbot renew --dry-run
```
```bash
sudo certbot renew
```