
## Setup MongoDB on VPS


Install MongoDB

```bash
  sudo apt-get install gnupg curl
```
```bash
  curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
Create the MongoDB list file.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
Update package lists and install MongoDB.

```bash
  sudo apt-get update
```

```bash
  sudo apt-get install -y mongodb-org
```
Start and enable MongoDB service.

```bash
  sudo systemctl daemon-reload
```
```bash
  sudo systemctl start mongod
```
```bash
  sudo systemctl enable mongod
```
Verify that MongoDB has started successfully.
```bash
  sudo systemctl status mongod
```
MongoDB URI to connect with projects.
```bash
  mongodb://127.0.0.1:27017
```

### Configure MongoDB Authentication ( Optional )

#### Allowing MongoDB through firewall

Enable firewall

```bash
sudo ufw enable
```

Check Port is Allowed through Firewall
```bash
  sudo ufw status
```

If Port is not Allowed then Allow it through Firewall
```bash
  sudo ufw allow 27017
```
```bash
   sudo ufw allow 'OpenSSH'
```
Restart MongoDB
```bash
  sudo service mongod restart
```
#### Secure MongoDB by setting up Super User. 

Connect to MongoDB shell
```bash
  mongosh
```
Show database

```bash
  show dbs
```

Change to admin database

```bash
  use admin
```

Create superuser with all privileges

```bash
  db.createUser({user: "username-here" , pwd: passwordPrompt() , roles: ["root"]})
```

Now Exit mongosh shell

```bash
  .exit
```

Enable Authorization removing comment

```bash
  sudo nano /etc/mongod.conf
```

```bash
  security:
    authorization: enabled
```
Restart MongoDB
```bash
  sudo service mongod restart
```

To Access Mongo Shell as Super User use this command:
```bash
  mongosh --port 27017 --authenticationDatabase "admin" -u "username-here" -p "password-here"
```

Create Database & User for project:
```bash
  use database_name
```
```bash
  db.createUser({user:"username_here", pwd:passwordPrompt(), roles:[{role:"readWrite", db:"database_name"}]})
```

Now Exit mongosh shell

```bash
  .exit
```

MongoDB URI to Connect with projects:
```bash
  mongodb://username-here:password-here@127.0.0.1:27017/database_name
```
