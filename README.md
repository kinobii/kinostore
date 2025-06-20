
# 📦 Kinostore – Online Gadget Store
https://kinostore.store/

IP address: 3.27.61.53

Video explainer: https://echo360.net.au/media/098fd6aa-c66a-47df-88c7-5748379d9874/public

Kinostore is a custom-built online store that showcases a range of affordable gadgets. It features category filters, search, product detail pages, cart management, and checkout — all hosted on an Amazon EC2 instance.

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
   - Open your terminal. Make sure you are in the directory where your key file is located for an easier way to SSH into your virtual Machine. The key file is in the Downloads directory, so the directory was changed to Downloads.
```bash
cd Downloads
```
By doing this, I won't have to write down the path to the key file, which can be a longer command and can be confusing or cause a typo.

The following command was used to set the key file’s permissions to read-only for the owner, and no permissions for anyone else.
```bash
chmod 400 kinokey.pem
```
Now that we have permission, we can SSH into the VM. To do so, the following command is executed:

eg., `ssh -i [keyfile].pem ubuntu@your-ip-address`

```bash
#mine
ssh -i kinokey.pem ubuntu@3.27.61.53
```

---

### 3️⃣ **Install Apache or Nginx Web Server**
Once inside the VM, install Apache or Nginx Web server.
In my case, I used Nginx, but it's the same thing but stick to one.
I used the following command to install Nginx

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
`product.html` and `checkout.html`, all written in Microsoft Visual Studio.

I also created a folder that has my `logo` image and all the images for the products that are to be listed on my website.

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

**note**: If the permission is denied, always try using `sudo`. (It doesn't hurt to do it)
Now we can upload the files that are inside the Kinostore folder to the server.

From the Mac terminal(new window, not inside the VM):

```bash
# Navigate to the folder
cd ~/Download

# Upload files
 scp -i kinokey.pem -r Kinostore ubuntu@3.27.61.53:~
```
Now the Kinostore folder has been copied to the virtual machine.
Next step, we have to move the files which are inside Kinostore to /var/www/html/ directory

```bash
mv Kinostore/* /var/www/html/
```

The '*' is used in this command as we are copying only the files inside the Kinostore folder.

---

### 6️⃣ **Access the Website**

Open a browser:

```
http://3.27.61.53/
```
Now we have a working online store!

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
## 📜 Custom Script Implementation
```bash
## 🧠 JavaScript Functionality Breakdown

- `displayProducts()`: Dynamically displays filtered products with query parameters.
- `addToCart()`: Adds selected product to cart and stores in localStorage.
- `updateCart()`: Renders cart items and updates count/total dynamically.
- `toggleCart()`: Toggles sidebar cart view.
- `checkoutCart()`: Validates and redirects to checkout page with saved cart.

```
🎯 Purpose & Relevance
This JavaScript script forms the core functionality of Kinostore. It powers product display, search and category filtering, cart management, and checkout redirection. Without this script, the website would be static and non-functional. The implementation was written from scratch without copying lab examples or online templates.

🛠️ How It Was Built
The script was entirely written by hand using vanilla JavaScript. It integrates deeply with HTML to control product display and cart interactions. Each function is tied to event listeners or DOM elements for dynamic behavior.

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
```bash
<script>
    const products = [
  { name: "Smart Plug", category: "smart-home", price: 12.99, img: "images/sp.png" },
  { name: "LED Light Strip", category: "lighting", price: 14.99, img: "images/lls.png" },
  { name: "Mini Tripod", category: "accessory", price: 9.99, img: "images/mtp.png" },
  { name: "Phone Holder", category: "accessory", price: 7.99, img: "images/ph.png" },
  { name: "Bluetooth Tracker", category: "smart-home", price: 18.49, img: "images/bt.png" },
  { name: "Portable Charger", category: "accessory", price: 24.95, img: "images/pc.png" },
  { name: "Smart Watch", category: "smart-home", price: 49.99, img: "images/sw.png" },
  { name: "Wireless Earbuds", category: "audio", price: 39.99, img: "images/we.png" },
  { name: "Bluetooth Speaker", category: "audio", price: 29.99, img: "images/bs.png" },
  { name: "Ring Light", category: "lighting", price: 22.49, img: "images/rl.png" }
];

```
 
---

## 🧾 How Checkout Works

1. User adds items to cart
2. Presses floating cart button
3. Clicks `Checkout`
4. Redirects to `checkout.html`
5. Fills out billing form and submits (form is validated)

---

## 🔒 Enable HTTPS with Certbot

The website has only been working using HTTP till now.
which will show as `not secure` in the browser

<img src="https://github.com/user-attachments/assets/f56032a8-1fb8-4715-8a62-0db26aea3f53" width="50%" height="50%" />



Execute the following command to allow HTTPS 
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx

```
---

## 🌐 Configure Domain Name
You can choose from a variety of domain providers like Namecheap, GoDaddy, etc., where you can get domains for as little as a dollar.

To configure the domain name using Namecheap, follow these steps:

Go to the Domain List section in your Namecheap dashboard.

Click the Manage button next to your domain name.

Click on the Advanced DNS tab.

Under Host Records, add a new A Record:

Host: @

Value: your EC2 public IP address (e.g., mine - 3.27.61.53)

TTL: Automatic or 30 minutes

Save the changes.

<img width="731" alt="Screenshot 2025-06-09 at 13 48 43" src="https://github.com/user-attachments/assets/54cda53a-8a94-4c77-bd99-4e892a5e7cc4" />


After DNS propagation (which may take a few minutes to several hours), visiting your domain (e.g., https://kinostore.store) will open your deployed website hosted on the EC2 instance.

<img src="https://github.com/user-attachments/assets/b30a9110-c5c9-4ee6-b4a4-3fdf19d692d3" width="50%">



---

## 👨‍🎓 Author

Kinley Phuntsho  
Student Number: 34645856 

Bachelor of Information Technology – Cybersecurity & Forensics  
Murdoch University, 
Semester 1, 2025  

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE). See the `LICENSE` file for details.

