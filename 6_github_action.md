# 🚀 Auto Deploy to VPS using GitHub Actions (CI/CD)

---

## ✅ Steps to Set Up

---

### 📁 Step 1: Create Workflow File

Create a file at `.github/workflows/deploy.yml` with the following content:

```yaml
name: Deploy to VPS

on:
  push:
    branches:
      - staging # Change to your branch name

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.VPS_SECRET_KEY }}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan -H ${{ secrets.VPS_HOST }} >> ~/.ssh/known_hosts

      - name: Deploy application
        run: |
          ssh -o ServerAliveInterval=60 -o StrictHostKeyChecking=no ${{ secrets.VPS_USER }}@${{ secrets.VPS_HOST }} << 'EOF'
            cd /var/www/admin-panel # Change to your project directory
            git pull origin staging
            export PATH=$PATH:/home/ubuntu/.nvm/versions/node/v22.14.0/bin  # Add Node.js to PATH
            npm install
            pm2 restart project-name || pm2 start src/index.js --name project-name
            pm2 save
            sudo systemctl restart nginx
          EOF
```

---

### 🔐 Step 2: Add GitHub Secrets

Go to your GitHub repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

Add the following secrets:

| Name             | Value                            |
|------------------|----------------------------------|
| `VPS_SECRET_KEY` | Your VPS private SSH key         |
| `VPS_HOST`       | Your VPS IP address              |
| `VPS_USER`       | Your VPS username (e.g. `ubuntu`) |


### 🚀 Step 3: Push Code to Deploy

```bash
git add .
git commit -m "Deploy update"
git push origin staging # Change to your branch name
```

That’s it! GitHub Actions will auto-deploy the latest code to your VPS 🎉
