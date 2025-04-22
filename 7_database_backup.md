
# How to Backup Your Database on a VPS


## Step 1: Access Your VPS
```bash
  ssh root@your_vps_ip
```


## Step 2: MongoDB Backup Command
```bash
  mongodump --uri="mongodb://username:password@localhost:27017/dbname" --out /backup/mongodb-$(date +\%Y-\%m-\%d) # change username, password, dbname
```


## Step 3: Backup file Compress (ZIP)
```bash
  tar -czvf /backup/mongodb-$(date +\%Y-\%m-\%d).tar.gz /backup/mongodb-$(date +\%Y-\%m-\%d)
```



## Step 4: Delete backup file
```bash
  rm -rf /backup/mongodb-$(date +\%Y-\%m-\%d)
```




# Upload Backup to AWS S3

## Step 5: Install AWS CLI
```bash
  sudo apt update

  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

  unzip awscliv2.zip

  sudo ./aws/install

  aws --version
```


## Step 6: Configure AWS CLI
```bash
  aws configure

    # Enter your AWS Access Key ID
    # Enter your AWS Secret Access Key
    # Enter your AWS Default region name
    # Enter your AWS Default output format (json)
```


## Step 7: Upload Backup to S3
```bash
  aws s3 cp /backup/mongodb-$(date +\%Y-\%m-\%d).tar.gz s3://your-s3-bucket-name/ # change your-s3-bucket-name
```


## Step 8: Setup Corn Job
```bash
  crontab -e

# Example: Backup MongoDB every 3 days
  0 0 */3 * * /bin/bash /home/user/mongodb_backup.sh
  
```


## Step 9: Check /home/user/mongodb_backup.sh
```bash
  ls /home/
  

# if not found, user
  sudo mkdir -p /home/user

# If Exist User
    sudo nano /home/user/mongodb_backup.sh
```


## Step 10: Pest in this code
```bash
#!/bin/bash

# MongoDB Backup Directory
BACKUP_DIR="/home/user/backups"
DATE=$(date +\%Y-\%m-\%d)
DB_NAME="your_actual_db_name"  # Mongodb Database Name
DB_URI="mongodb://username:password@localhost:27017/$DB_NAME"  # Mongodb URI with username, password, and database name

# MongoDB Backup Command
mongodump --uri="$DB_URI" --out $BACKUP_DIR/mongodb-$DATE 

# Compress Backup
tar -czvf $BACKUP_DIR/mongodb-$DATE.tar.gz $BACKUP_DIR/mongodb-$DATE
rm -rf $BACKUP_DIR/mongodb-$DATE

# Upload to AWS S3
aws s3 cp $BACKUP_DIR/mongodb-$DATE.tar.gz s3://your-s3-bucket-name/  # S3 bucket name

# Remove Local Backup
rm -rf $BACKUP_DIR/mongodb-$DATE.tar.gz

echo "MongoDB Backup Completed and Uploaded to S3 on $DATE"
```



## Step 9: Make the file executable
```bash
  chmod +x /home/user/mongodb_backup.sh
```