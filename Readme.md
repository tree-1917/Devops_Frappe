---
Source A: https://docs.frappe.io/framework/user/en/tutorial/install-and-setup-bench
Source B: https://discuss.frappe.io/t/guide-how-to-install-erpnext-v15-on-linux-ubuntu-step-by-step-instructions/111706
---

---
## Frappe 101 

### Install Frappe and It's Needs Parts 

> Installation Command 

```bash 
# Arch 
sudo pacman -S yarn wkhtmltopdf python redis nginx git

# Redhat 
sudo dnf install yarn wkhtmltopdf python redis nginx git

# Ubuntu 
sudo apt install wkhtmltopdf python redis nginx yarn  git
```

> Install bench 

```bash
# Arch 
yay -S unixbench

# Redhat 

# Ubuntu (pipx)
sudo pipx install frappe-bench
```
### Start a New Project 

> Start New Project 

```shell
$ bench init frappe-hr
```

> Source `env`

```bash 
$ source env/bin/activate
```

> Create a New Site 

```bash
bench new-site mysite.local
# Will Ask You About Maridb `User` and `Password`
```

> Install `ERPNext` App 

```bash
# install latest erpnext 
bench get-app erpnext 
# specify erpnext branch 
bench get-app --branch version-15 erpnext
```

> Assign This Apps To Your Site 

```bash
bench --site mysite.local install-app erpnext
```

> Define Default Site in Bench 

```bash
bench use mysite.local
```

> Start Bench **Development**

```bash
bench --site mysite.local serve --port 9000
```

> Start Bench  **Produciton**

```bash 
bench start --port 9000 # this need supervisorctl work it's usee it 
```

### Admin Account 

> Default `Administrator` account 

```bash 
bench --site mysite.local set-admin-password
# UserName: Administrator
# Pass: <Need To Set It>
```

> Create `Administrator` account From  `console` 

```bash
# Open Frappe Console 
bench --site mysite.local console
```

> Create A Admin User

```python
import frappe

# Create a new user
user = frappe.get_doc({
    "doctype": "User",
    "email": "newadmin@example.com",
    "first_name": "New",
    "last_name": "Admin",
    "enabled": 1,
    "send_welcome_email": 0
})

user.insert(ignore_permissions=True)
frappe.db.commit()
```

> Set Password For User 

```python 
from frappe.utils.password import update_password

update_password("newadmin@example.com", "StrongPassword123")
```

> Update Permission For User 

```python 
# Load the user again
u = frappe.get_doc("User", "newadmin@example.com")

# Assign System Manager role
u.append("roles", {"role": "System Manager"})
u.save()
frappe.db.commit()
```
> Print `Admin` User 

```python
u = frappe.get_doc("User", "admdin@xyz.com")
print(u.as_dict())
```
> Print `All` Users

```python 
# Print Only Names 
frappe.get_all("User", pluck="name")

# Print Name, Full Names, Email, Enabled Status 
users = frappe.get_all("User", fields=["name", "full_name", "email_id", "enabled"])
for u in users:
    print(u)
```

> Check Redis Work

```bash
redis-server --port 13000 &
redis-server --port 11001 &
redis-server --port 12001 &
```

> Check  Redis

```bash
redis-cli -p 11001 ping
redis-cli -p 12001 ping 
redis-cli -p 13000 ping
```

### Make it Works on Local Network With `supervisorctl`

```bash

```
### Make More Tow Site Works at different ports 

### Link Database Tire With Frappe Tire 

> Default Configuration For Replication 

```json
// site_config.json //
{
  "db_name": "frappe_db",
  "db_password": "MasterStrongPassword",
  "db_type": "mariadb",
  "db_user": "frappe_master",

  "db_host": "10.0.0.10",

  "read_replica": {
    "host": "10.0.0.11",
    "port": 3306,
    "user": "frappe_replica",
    "password": "ReplicaPassword"
  }
}
```

> Test Replication 

```python
# Enter in Frappe Site Console And Run # 
import frappe
from frappe.database.mariadb.database import MariaDBDatabase

db = frappe.db

print("Master host:", db.host)

# Replica test:
replica_conn = frappe.db.get_read_replica_connection()
print("Replica host:", replica_conn.host)
```