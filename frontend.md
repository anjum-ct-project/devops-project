Objective
To configure and deploy the frontend module of the Food Expense Application using nginx as the web server to serve static web content.

Prerequisites
A RHEL-based Linux server with internet access.
Sudo privileges on the system.
Backend and Database components deployed (for final integration).

Step-by-Step Instructions

Install Nginx Web Server
The frontend service is built as a static web application. To serve it, we'll use Nginx, a lightweight and efficient web server.
dnf install nginx -y

Start and Enable Nginx
This ensures that Nginx starts now and automatically starts on system boot.
systemctl enable nginx
systemctl start nginx

Clean Default Web Content
We will remove the default web files so we can place our application content instead.
rm -rf /usr/share/nginx/html/\*

Download and Extract Frontend Content
Download the pre-built frontend content from the artifact storage and extraxt it into Nginx's web directory.
curl -o /tmp/frontend.zip https://expense-artifacts.s3.amazonaws.com/expense-frontend-v2.zip

cd /usr/share/nginx/html
unzip /tmp/frontend.zip

Configure Nginx as a Reverse Proxy
Create a new configuration file to handle API and health routes correctly.
vim /etc/nginx/default.d/expense.conf
Paste the following configuration:
proxy_http_version 1.1;

location /api/ {
proxy_pass http://<BACKEND-IP>:8080/;
}

location /health {
stub_status on;
access_log off;
}
Note: Replace the <BACKEND-IP> with the actual IP address of your backend server.

Restart Nginx to Apply Changes
systemctl restart nginx
