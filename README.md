# Set up Taxi
This is the step-by-step installation instructions of AirportTaxi.

# Prerequisites
- docker
- docker-compose

## Step 1: Clone this repo
```
git clone git@github.com:quangtuan9237-ht/my-taxi-setup.git
cd my-taxi-setup
```

## Step 2: Clone source code
Clone source code to folder `my-taxi-setup`
```
git clone git@github.com:InboundPlatform/WifiAPI.git
git clone git@github.com:InboundPlatform/WifiAdmin.git
git clone git@github.com:InboundPlatform/AirportTaxi.git
```

## Step 3: Start services
```
docker-compose up -d mysql
docker-compose up -d redis
docker-compose up -d phpmyadmin
docker-compose up -d mail
```

## Step 3: Create DB using phpmyadmin
- Go to `http://localhost:8080/`
- Login with this info:
```
Server: mysql
Username: root
Password: password
```
- Create a DB with name `wifi_api`

## Step 4: Setup wifi API
Start the API container
```
docker-compose up -d wifi_api
```
Attach CLI to the created container
```
docker-compose exec wifi_api bash
```
Execute commands inside the container
```
composer install
php artisan migrate --seed
chmod 777 -R storage bootstrap
```

## Step 5: Setup wifi admin
Start the WifiAdmin container
```
docker-compose up -d wifi_admin
```
Attach CLI to the created container
```
docker-compose exec wifi_admin bash
```
Execute commands inside the container
```
composer install
chmod 777 -R storage bootstrap
```

## Step 6: Setup AirportTaxi
Start the AirportTaxi container
```
docker-compose up -d taxi
```
Attach CLI to the created container
```
docker-compose exec taxi bash
```
Execute commands inside the container
```
composer install
chmod 777 -R storage bootstrap
```
Comment out lines in .htaccess:
```
RewriteCond %{HTTP:X-Forwarded-Proto} !=https
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```
#### Tip: 
To exclude .htaccess out git tracking so you will not accidentally commit the file, run command:
```
cd AirportTaxi
git update-index --assume-unchanged public/.htaccess
```

## Step 7: Access the web:
- Taxi: `https://localhost:4455/`
- Email service: `http://localhost:8025/`
- phpmyadmin: `http://localhost:8080/`
```
Server: mysql
Username: root
Password: password
```
- WifiAdmin: `https://localhost:4400/admin`
```
Email: admin003@japan-wireless.com
Initial passworld: Aa123456@
```
#### Tip: 
To reset password to the initial one, go to phpmyadmin, table `users`, update password hash of the desire email
```
$2y$10$c5/seauXruCDDy40gAbrIum3AgtD00NyU.c/zpTwkB05FeAys943C
```
