
# 📦 Kinostore – Online Gadget Store

Kinostore is a custom-built, fully functional online store that showcases a range of affordable gadgets. It features category filters, search, product detail pages, cart management, and checkout — all hosted on an Amazon EC2 instance.

---

## ✅ Project Overview

- **Platform**: Amazon EC2 (Ubuntu)
- **Stack**: HTML, CSS, JavaScript
- **Web Server**: Nginx 
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
   - **Key pair**: Create a new key - kinokey.pem
        ~ the .pem file will be downloaded after that.
   - **Security Group**: Allow HTTP (80), HTTPS (443), SSH (22)
   - Don't worry about all the other buttons and launch the instance.
     
3. Note the **public IP**
      mine - 3.27.61.53


### 2️⃣ **Connect via SSH from Terminal**
   - Open your terminal. Make sure you are in the directory where your .pem file is located, for an easier way to SSH into your virtual Machine. I can find the .pem file in the Downloads directory, so the directory was changed to Downloads.
```bash
cd Downloads
```
By doing this, I won't have to write down the path to the .pem file, which can be a longer command and can be confusing or cause a typo.

The following command was used to set the file’s permissions to read-only for the owner, and no permissions for anyone else.
```bash
chmod 400 kinokey.pem
```
SSH into the VM
```bash
ssh -i kinokey.pem ubuntu@3.27.61.53
```

---

### 3️⃣ **Install Apache Web Server**
Once inside the VM, install Apache or Nginx Web server.
In my case, I used Nginx, but it's the same thing.
I used the following command to install apache

```bash
sudo apt update
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

Tested it in browser: `http:/3.27.61.53/`
The browser shows a page saying the website works!

---

### 4️⃣ **Prepare Website Files**

Locally on my Mac(can be PC)
Wrote basic HTML, CSS, and JavaScript for the website in Microsoft Visual Studio.

**note**: Using a text editor can be helpful, as the terminal text editor does not provide flexibility.
The index.html file is the main web page, which shows up when you search your IP address in a browser.

As an online store, I created 2 other HTML files aside from index.html, namely 
product.html and checkout.html, all written in Microsoft Visual Studio.

I also created a folder that has my logo image and all the images for the products that are to be listed on my website.

The files are all on my Desktop inside a folder called Kinostore. 

 ```
Kinostore/
|   ├── index.html
|   ├── checkout.html
|   ├── product.html
|   ├── images/
|   │   ├── bs.png         # Bluetooth Speaker
|   │   ├── bt.png         # Bluetooth Tracker
|   │   ├── lls.png        # LED Light Strip
|   │   ├── mtp.png        # Mini Tripod
|   │   ├── pc.png         # Portable Charger
|   │   ├── ph.png         # Phone Holder
|   │   ├── rl.png         # Ring Light
|   │   ├── sp.png         # Smart Plug
|   │   ├── sw.png         # Smart Watch
|   │   ├── we.png         # Wireless Earbuds
|   │   └── logo1.png      # Website logo + favicon

 ```
---

### 5️⃣ **Upload to EC2 Server**

Before we upload the folder to the server, I deleted the index.html file, which was already in the /var/www/html directory, to avoid duplicate files.
 The following command was used while inside the VM
 ```bash
cd /var/www/html
rm index.html
```
Now we can upload the files that are inside the Kinostore folder to the server.

From the Mac terminal(new window, not inside the VM):

```bash
# Navigate to the folder
cd ~/Desktop

# Upload files
scp -i ~/.ssh/kinokey.pem -r Kinostore/* ubuntu@3.27.61.53:/var/www/html/
```

The '*' is used in this command as we are copying only the files inside the Kinostore folder.

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

**Linking the pages**:

The `product.html` and `checkout.html` pages are linked to `index.html`. When a user clicks on a product card (excluding the "Add to Cart" button), they are redirected to `product.html`, which displays the detailed product description using query parameters passed in the URL. The "Checkout" button in the floating cart sidebar redirects users to `checkout.html`, where they can review their order, enter customer details, select shipping methods, and provide payment information.

The images are linked as follows(index.html):
<img width="643" alt="Screenshot 2025-06-09 at 12 27 44" src="https://github.com/user-attachments/assets/9a2c9f85-86f1-4e4f-ad5a-ce21dc8e3d47" />
 
---

## 🧾 How Checkout Works

1. User adds items to cart
2. Presses floating cart button
3. Clicks `Checkout`
4. Redirects to `checkout.html`
5. Fills out billing form and submits (form is validated)

---

## 🔒 Enable HTTPS with Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx

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
