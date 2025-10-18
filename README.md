AWS + NGINX Deployment step-by-Step Journal 
---

<img width="940" height="503" alt="image" src="https://github.com/user-attachments/assets/ddcf63ae-8bdd-42b6-ae75-f277204185d8" />


--- 
 

## Step 1 — Buy a Domain on Cloudflare 
---


Log in to Cloudflare.

Go to “Register a Domain”.

Search for your desired domain (e.g., mywebsite.com).

Complete the purchase and verify ownership.

<img width="940" height="180" alt="image" src="https://github.com/user-attachments/assets/114e88c5-6826-496a-8f0a-cd7215ac4358" />

--- 
 

## Step 2 — Launch an EC2 Instance 
---

Log in to AWS Console → EC2 → Launch Instance.

Select Amazon Linux 2 AMI.

Choose t2.micro for free tier.

Configure security group:

SSH → your IP

HTTP → anywhere

HTTPS → anywhere

Create/download the key pair (.pem).


<img width="940" height="353" alt="image" src="https://github.com/user-attachments/assets/c66d5f3c-42b6-46b3-a476-00289c1e0d2d" />


--- 

## Step 3 — Connect to the EC2 Instance via SSH 
---


Open terminal.

Set permissions for your key:

chmod 400 my-key.pem

Connect via SSH:


ssh -i "my-key.pem" ec2-user@<EC2_PUBLIC_IP>


<img width="940" height="452" alt="image" src="https://github.com/user-attachments/assets/d72074dc-1c1e-4e60-b5f1-6c3a37937bc9" />


--- 
 

## Step 4 — Install & Start NGINX on EC2 
---

Run the following commands after SSH-ing in: 

`sudo yum update –y`  

`sudo yum install -y nginx`  

`sudo systemctl start nginx`  

`sudo systemctl enable nginx`  

`sudo systemctl status nginx`  # should show "active (running)"   

Test: 
Open http://<YOUR_EC2_PUBLIC_IP> in your browser — you should see the NGINX welcome page. 


<img width="818" height="342" alt="image" src="https://github.com/user-attachments/assets/87dcd756-4f5b-46b5-95ba-ce1e98ce58a7" />



--- 

## Step 5 — Point Your Domain to the EC2 IP (Cloudflare) 
---

Go to Cloudflare → Your Domain → DNS → Add record. 

Add: 

Type: A 

Name: @ (root) or nginx (for nginx.yourdomain.com) 

IPv4 Address: <EC2_PUBLIC_IP> 

TTL: Auto 

Proxy Status: DNS only (grey cloud) for initial testing 

Save. 


<img width="940" height="216" alt="image" src="https://github.com/user-attachments/assets/c6ab8c4f-f3ce-4241-be38-87c2abe09009" />




--- 
 
## Step 6 — Confirm DNS is Live 
---

From your local machine: 

'nslookup your.domain`  

or 

`dig +short your.domain` 

 
You should see your EC2 public IP. 

 
Visit http://(your.domain) — the default NGINX page should now appear. 

---

## Step 7 — Replace the Default Page with a Custom One 
---
 
 
Navigate to the web root and back up the default file: 
 
`cd /usr/share/nginx/html`  

`sudo cp index.html index.html.bak` 
 
## Edit the page: 
---

`sudo nano index.html`  

 
Delete the default content, paste in your own HTML code.

Save (CTRL+O, Enter) and exit (CTRL+X).


<img width="940" height="503" alt="image" src="https://github.com/user-attachments/assets/83aa3ae8-d11a-4d83-8b14-a0d145c77dac" />


--- 

Reflection
---

Learned EC2, NGINX, and Cloudflare integration.

Successfully deployed a personal webpage.

Next steps: add SSL, create multiple pages, explore server blocks.















