# GSP306 : Migrate a MySQL Database to Google Cloud SQL

```bash
export ZONE=us-central1-a
```

```bash
gcloud sql instances create wordpress --tier=db-n1-standard-1 --activation-policy=ALWAYS --gce-zone $ZONE
```

```bash
gcloud sql users set-password --host % root --instance wordpress --password Gsp306*
```

```bash
export ADDRESS=[external IP of blog vm instance]/32
```

```bash
gcloud sql instances patch wordpress --authorized-networks $ADDRESS --quiet
```

```bash
gcloud compute ssh blog --zone=us-central1-a
```

```bash
MYSQLIP=$(gcloud sql instances describe wordpress --format="value(ipAddresses.ipAddress)")
```

```bash
mysql --host=[INSTANCE_IP_ADDRESS] \
 --user=root --password
```

```SQL
CREATE DATABASE wordpress;
CREATE USER 'blogadmin'@'%' IDENTIFIED BY 'Gsp306*';
GRANT ALL PRIVILEGES ON wordpress.* TO 'blogadmin'@'%';
FLUSH PRIVILEGES;
```

```
exit
```

```
sudo mysqldump -u root -pGsp306* wordpress > wordpress_backup.sql
```

```
mysql --host=$MYSQLIP --user=root -pGsp306* --verbose wordpress < wordpress_backup.sql
```

```
sudo service apache2 restart
```

```
cd /var/www/html/wordpress
```

```
sudo nano wp-config.php
```
