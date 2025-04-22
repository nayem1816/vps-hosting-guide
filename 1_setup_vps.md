
# Setup your VPS (Ubuntu) Server


## Step 1: Access Your VPS
```bash
  ssh root@your_vps_ip
```


## Step 2: Update Your Server
```bash
  sudo apt update && sudo apt upgrade -y
```


## Step 3: Install Node.js and npm
```bash
  # Download and install nvm:
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

  # in lieu of restarting the shell
  \. "$HOME/.nvm/nvm.sh"

  # Download and install Node.js:
  nvm install 22

  # Verify the Node.js version:
  node -v # Should print "v22.14.0".

  nvm current # Should print "v22.14.0".

  # Verify npm version:
  npm -v # Should print "10.9.2".
```


## Step 4: Install Git
```bash
  sudo apt install -y git
```


## Step 5: Install Nginx 
```bash
  sudo apt install nginx -y
```


## Step 6: Install pm2 
```bash
  npm install -g pm2
```