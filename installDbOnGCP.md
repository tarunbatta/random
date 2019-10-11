# Install Db on Google Cloud (Debian 8)

## Install MySql

1. Update and Upgrade the system
    * sudo apt update && sudo apt upgrade -y
1. Install mysql
    * sudo apt-get -y install mysql-server
1. Improve mysql installation security
    * sudo mysql_secure_installation
1. Fix the bind address
    * sudo nano /etc/mysql/my.cnf
1. Change bind address from From 127.0.0.1 to 10.138.0.4
    * sudo /etc/init.d/mysql restart
1. Access mysql
    * sudo mysql -u root -p
    * GRANT ALL ON *.* to root@'10.138.0.4' IDENTIFIED BY '^Ou2Bhux0xcTSwlG%iDx';

## Install MongoDb

1. Update and Upgrade the system
    * sudo apt update && sudo apt upgrade -y
1. Install [MongoDb](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/)
    * sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    * echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    * sudo apt-get update
    * sudo apt-get install -y mongodb-org
    * sudo systemctl enable mongod.service
    * sudo service mongod start
1. Give permissions to folders
    * sudo chown mongodb:mongodb /var/lib/mongodb
    * sudo chown mongodb:mongodb /var/log/mongodb
1. Enable mongod service
    * sudo systemctl enable mongod.service
    * sudo systemctl status mongod.service
1. Change the Bind IP to cloud internal IP
    * sudo nano /etc/mongod.conf
    * From 127.0.0.1 to 10.138.0.4
1. Create admin user
    * mongo
    * use admin
    *   ```
        db.createUser(
        {
            user: "admin",
            pwd: "8@fYcY2bkyxtzz7Y^ea5",
            roles: [ { role: "root", db: "admin" } ]
        })
        ```
1. Change admin password
    * mongo
    * use admin
    * db.changeUserPassword("admin", "3eAryYayjKvvWrgN5YQ5")
1. Add security to mongod config
    * sudo nano /etc/mongod.conf
    *   ```
        security:        
            authorization: enabled
        ```
    *   ```
        setParameter:        
            enableLocalhostAuthBypass: false
        ```
1. Create data/db folder,
    * sudo mkdir -p /data/db
    * sudo chown -R $USER /data/db
1. Restart mongod service
    * sudo service mongod restart
