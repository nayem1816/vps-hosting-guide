
# 💻 GitHub SSH Setup on VPS

এই ডকুমেন্টেশনে দেখানো হয়েছে কীভাবে VPS-এ SSH এর মাধ্যমে GitHub এর সাথে কানেকশন সেটআপ করে প্রজেক্ট ক্লোন, পুশ ও পুল করা যায়।

---

## 📆 Step 1: Access Your VPS
```bash
ssh root@your_vps_ip
```

---

## 🛠️ Step 2: Update Your Server
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🧰 Step 3: Install Git
```bash
sudo apt install -y git
```

### Git ইনস্টল আছে কিনা চেক করা:
```bash
git --version
```

---

## 🧾 Step 4: Git Username & Email কনফিগার করা
```bash
git config --global user.name "user name"
git config --global user.email "email@gmail.com"
```

---

## 🔐 Step 5: SSH Key আছে কিনা চেক করা
```bash
ls -al ~/.ssh
```

### নতুন SSH Key জেনারেট করতে (যদি না থাকে):
```bash
ssh-keygen -t ed25519 -C "email@gmail.com"
```
> Default লোকেশন আর পাসফ্রেস স্কিপ করতে `Enter` চাপ দিন।

---

## 🚀 Step 6: SSH Agent চালু করা ও Key অ্যাড করা
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 📋 Step 7: SSH Public Key GitHub-এ Add করা
```bash
cat ~/.ssh/id_ed25519.pub
```

1. উপরের কমান্ড দিয়ে key কপি করুন।
2. GitHub এ যান: https://github.com/settings/ssh/new
3. Title দিন: `My VPS Key`
4. Key ফিল্ডে paste করুন
5. **Add SSH Key** বাটনে ক্লিক করুন

---

## ✅ Step 8: GitHub এর সাথে SSH কানেকশন টেস্ট করা
```bash
ssh -T git@github.com
```
সফল হলে এররকম মেসেজ দেখাবে:
```
Hi brandtechbd! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 📂 Step 9: প্রজেক্ট ক্লোন করা (SSH URL দিয়ে)
```bash
git clone git@github.com:BrandTechDev/affsflow-server.git
```

---