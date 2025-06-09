
# 📦 Kinostore – Online Gadget Store

Kinostore is a custom-built, fully functional online store that showcases a range of affordable gadgets. It features category filters, search, product detail pages, cart management, and checkout — all hosted on an Amazon EC2 instance.

---

## ✅ Project Overview

- **Platform**: Amazon EC2 (Ubuntu)
- **Stack**: HTML, CSS, JavaScript
- **Web Server**: Apache2
- **Secure Access**: SSH (using `.pem` key)
- **Cart & Checkout**: JavaScript + `localStorage`
- **Deployment**: Manual upload via `scp` from macOS terminal

---

## 🛠️ Step-by-Step: How It Was Built

### 1️⃣ **Create and Set Up the EC2 VM**

1. Go to [AWS EC2 Dashboard](https://console.aws.amazon.com/ec2)
2. Launch a new instance:
   - **AMI**: Ubuntu 22.04 LTS
   - **Instance type**: t2.micro (free tier)
   - **Key pair**: Create or select `kinokey.pem`
   - **Security Group**: Allow HTTP (80), HTTPS (443), SSH (22)
3. Note the **public IP** (e.g., `3.27.61.53`)

### 2️⃣ **Connect via SSH from Mac Terminal**

```bash
cd ~/.ssh
chmod 400 kinokey.pem
ssh -i kinokey.pem ubuntu@3.27.61.53
```

---

### 3️⃣ **Install Apache Web Server**

```bash
sudo apt update
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl start apache2
```

Test it in browser: `http://3.27.61.53`

---

### 4️⃣ **Prepare Website Files**

Locally on your Mac:
- Create a folder `Kinostore` on Desktop
- Inside it, place:
  - `index.html`
  - `checkout.html`
  - `product.html`
  - `images/` (folder with 10 product images + logo1.png)

---

### 5️⃣ **Upload to EC2 Server**

From your Mac terminal:

```bash
# Navigate to the folder
cd ~/Desktop

# Upload files
scp -i ~/.ssh/kinokey.pem -r Kinostore/* ubuntu@3.27.61.53:/var/www/html/
```

---

### 6️⃣ **Access the Website**

Open a browser:

```
http://3.27.61.53/
```

---

## 🧱 Folder Structure on Server

```
/var/www/html/
├── index.html
├── checkout.html
├── product.html
├── images/
│   ├── bs.png         # Bluetooth Speaker
│   ├── bt.png         # Bluetooth Tracker
│   ├── lls.png        # LED Light Strip
│   ├── mtp.png        # Mini Tripod
│   ├── pc.png         # Portable Charger
│   ├── ph.png         # Phone Holder
│   ├── rl.png         # Ring Light
│   ├── sp.png         # Smart Plug
│   ├── sw.png         # Smart Watch
│   ├── we.png         # Wireless Earbuds
│   └── logo1.png      # Website logo + favicon
```

---

## 💡 Features Included

- 🔍 Search bar and category filtering
- 🛍️ Cart functionality with sidebar + full screen toggle
- 📦 Product detail page for each item (`product.html`)
- ✅ Working checkout page with validation
- 📱 Responsive design and mobile compatibility
- 🌐 Favicon and branding integration
- 🎨 Smooth scrolling and animated interactions

---

## 🧾 How Checkout Works

1. User adds items to cart
2. Presses floating cart button
3. Clicks `Checkout`
4. Redirects to `checkout.html`
5. Fills out billing form and submits (form is validated)

---

## 🔒 (Optional) Enable HTTPS with Certbot

```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache
```

---

## 📤 GitHub Deployment Instructions

1. Create GitHub repository (e.g., `kinostore`)
2. Push your local project folder:

```bash
cd ~/Desktop/Kinostore
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/kinostore.git
git push -u origin master
```

---

## 👨‍🎓 Author

Kinley Phuntsho  
Bachelor of Information Technology – Cybersecurity & Forensics  
Murdoch University, 2025  
