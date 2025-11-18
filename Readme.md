---
Source A: https://docs.frappe.io/framework/user/en/tutorial/install-and-setup-bench
Source B: https://discuss.frappe.io/t/guide-how-to-install-erpnext-v15-on-linux-ubuntu-step-by-step-instructions/111706
---
# 🌟 **Frappe 101 — Complete Beginner Guide (Updated & Extended)**

This guide explains **how to install Frappe/ERPNext**, manage sites, run Frappe with **Supervisor**, configure **multiple ports**, and set up **NGINX reverse proxy** for production.

---

# 📌 1. Install Dependencies

Install required packages depending on your Linux distribution.

```bash
# Arch Linux  
sudo pacman -S yarn wkhtmltopdf python redis nginx git

# RedHat  
sudo dnf install yarn wkhtmltopdf python redis nginx git

# Ubuntu   
sudo apt install wkhtmltopdf python3 redis nginx yarn git
```

---

# 📌 2. Install Bench

```bash
# Arch  
yay -S unixbench

# Ubuntu  
sudo pipx install frappe-bench
```

---

# 📌 3. Create New Project

### Initialize bench:

```bash
bench init frappe-hr
```

### Activate virtualenv:

```bash
source env/bin/activate
```

### Create new site:

```bash
bench new-site mysite.local
```

---

# 📌 4. Install ERPNext

```bash
# Latest
bench get-app erpnext

# Or specific version
bench get-app --branch version-15 erpnext
```

Install it into the site:

```bash
bench --site mysite.local install-app erpnext
```

Set default site:

```bash
bench use mysite.local
```

---

# 📌 5. Run Bench

### Development

```bash
bench --site mysite.local serve --port 9000
```

### Production (Supervisor)

```bash
bench start
```

---

# 📌 6. Administrator Setup

Reset admin password:

```bash
bench --site mysite.local set-admin-password
```

---

# 📌 7. Redis Services

Frappe uses 3 Redis instances:

| Purpose  | Port  |
| -------- | ----- |
| Cache    | 13000 |
| Queue    | 11001 |
| SocketIO | 12001 |

Check Redis status:

```bash
redis-cli -p 11001 ping
redis-cli -p 12001 ping
redis-cli -p 13000 ping
```

---

# 📌 8. Run **Two Bench Servers** on Different Ports (Same Site)

### Why?

* Useful for development
* Useful for load balancing
* Useful for multi-instance setups

---

## 🟧 **Supervisor Config for PORT 9000**

Create:

```
/etc/supervisor/conf.d/frappe_9000.conf
```

```ini
[program:frappe_9000]
command=/usr/bin/bench --site mysite.local serve --port 9000
directory=/home/moussa/frappe-hr
autostart=true
autorestart=true
stderr_logfile=/var/log/frappe_9000.err.log
stdout_logfile=/var/log/frappe_9000.out.log
```

---

## 🟩 **Supervisor Config for PORT 9001**

Create:

```
/etc/supervisor/conf.d/frappe_9001.conf
```

```ini
[program:frappe_9001]
command=/usr/bin/bench --site mysite.local serve --port 9001
directory=/home/moussa/frappe-hr
autostart=true
autorestart=true
stderr_logfile=/var/log/frappe_9001.err.log
stdout_logfile=/var/log/frappe_9001.out.log
```

---

## 🔄 Reload Supervisor

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl restart frappe_9000
sudo supervisorctl restart frappe_9001
```

Now:

| Instance | Port | URL                                            |
| -------- | ---- | ---------------------------------------------- |
| Server A | 9000 | [http://localhost:9000](http://localhost:9000) |
| Server B | 9001 | [http://localhost:9001](http://localhost:9001) |

---

# 📌 9. Production NGINX Configuration (Recommended)

You can route requests to the two Frappe servers:

---

## 🌍 **NGINX Reverse Proxy (with two backend ports)**

Create file:

```
/etc/nginx/sites-enabled/frappe.conf
```

### 🔧 Basic Reverse Proxy

```nginx
server {
    listen 80;
    server_name mysite.local;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Use port 9000 as main backend
        proxy_pass http://127.0.0.1:9000;
    }
}
```

---

## 🔁 **NGINX Load Balancing between two Frappe instances**

```nginx
upstream frappe_backend {
    server 127.0.0.1:9000;
    server 127.0.0.1:9001;
}

server {
    listen 80;
    server_name mysite.local;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_pass http://frappe_backend;
    }
}
```

➡️ This will load balance between :

* **9000**
* **9001**

---

## 🟦 SocketIO (Real-time Events)

```nginx
server {
    listen 9002;
    server_name mysite.local;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## 🔄 Reload NGINX

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

# 📌 10. Multiple Sites / Multiple Ports

```bash
bench new-site site1.local
bench new-site site2.local

bench --site site1.local serve --port 9100
bench --site site2.local serve --port 9200
```

---

