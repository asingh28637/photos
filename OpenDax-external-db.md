## Using an external database in opendax

### Step 1.1: Installing mysql
`sudo apt update`

`sudo apt install mysql-server`


### Step 1.2: Configuring
`sudo mysql_secure_installation` #Set password here etc

`mysql -u root -p` #Input password set before when prompted.

Once you are in the mysql terminal, type/copy this:

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'YOURPASSWORD' WITH GRANT OPTION;`

When you are done, make sure to update:

`FLUSH PRIVILEGES;`



### Step 1.3: Allow connections from your main opendax server
`nano /etc/mysql/mysql.conf.d/mysqld.cnf`

Comment out the following lines
```
#bind-address           = 127.0.0.1
#skip-networking
```

`/etc/init.d/mysql restart`

### Step 2.1: Configuring on your main opendax server
Switch over to your main server. Under `config/app.yml` change your database options too:

```
database:
  host: IP of your server here
  port: 3306
  user: root
  password: password you set earlier
```

### Updating OpenDax to use the new settings and running

Run `rake render:config` to update all your configs (To make sure this worked, run `nano config/peatio.env` and the db details you just set should be there)

Make sure you have stopped all running containers:

`rake service:all[stop]`

Now start opendax up:

`rake service:all`