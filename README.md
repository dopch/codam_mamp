From Day 03 subject will mention a soft named PAMP (stack to run Apache, Mysql, Php)
Sadly PAMP has be developed a long time ago and only run on OSX El Capitan or below =(

For that reason Max and Bodo, have created a Docker Compose to replace PAMP (Lucky students) !

### DOCKER

Why ?

Because it's a good way to have a unique environnment across multiple operation system.
I invite you to read more technical article and manual about it.


# Installation

To use MAMP Docker-compose, no need to be a wizard , you only need to clone this repo and edit a configuration file to feet your need
Obviously, `Docker` is required, he is available in MSC just on the corner!

Clone the following repo in your home folder :

```sh
git clone https://github.com/dopch/codam_mamp
```

Here is the file&directory structure :

```
codam_mamp
└───config
│   └───php
│       └───7.0
│         │ php.ini
│         │
└───data
│   └───www
│     │ index.php
│     │
docker-compose.yml
```

`docker-compose.yml` is the most important file :


```yaml
version: '2'
services:
  web:
    image: keopx/apache-php:7.0
    ports:
      - "8008:80"
    links:
      - mysql
    volumes:
      - ./data/www/:/var/www/html
      - ./config/vhosts:/etc/apache2/sites-enabled
      - ./config/php/7.0/php.ini:/etc/php/7.0/apache2/php.ini
      - ./config/apache2:/etc/apache2
    working_dir: /var/www/html
    environment:
      - PHP_SENDMAIL_DOMAIN=mail-student.codam.nl:25
  mysql:
    image: keopx/mysql:5.5
    ports:
      - "3306:3306"
    volumes:
      - ./data/database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=sirsironenagros
      - MYSQL_DATABASE=kaamelott_db    
      - MYSQL_USER=perceval           
      - MYSQL_PASSWORD=Cestpasfaux
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    links:
      - mysql
    environment:
      - PMA_HOST=mysql
```

He contain the following info :
  * Which docker image will be used (`image: keopx/apache-php:7.0`)
  * Your local folder info and the path where he will be mounted/attach inside docker container (`./data/www:/var/www/html` format = your_local_path:Path_inside_container)
  * Your credentials for your sql database (` MYSQL_USER=testuser`)

The above points are the most intersting, fill free to discover the meaning of the others by yourself =)

### Configuration

Line you will need to edit :
```yaml
  - ./data/www:/var/www/html
```

This will be the folder where you drop your `.php` file.
We strongly advice you to change this path to point to your home folder(`/Users/login/piscine-php/MyWebSite`).

```yaml
- MYSQL_ROOT_PASSWORD=sirsironenagros
- MYSQL_DATABASE=kaamelott_db         
- MYSQL_USER=perceval           
- MYSQL_PASSWORD=Cestpasfaux
```
Above variable will be set when your container start.

### Exemple
Here is an example of a configuration for a user named `perceval`:
```yaml
version: '2'
services:
  web:
    image: keopx/apache-php:7.0
    ports:
      - "8008:80"
    links:
      - mysql
    volumes:
      - /Users/perceval/piscine-php/MyWebSite:/var/www/html
      - ./config/vhosts:/etc/apache2/sites-enabled
      - ./config/php/7.0/php.ini:/etc/php/7.0/apache2/php.ini
      - ./config/apache2:/etc/apache2
    working_dir: /var/www/html
    environment:
      - PHP_SENDMAIL_DOMAIN=mail-student.codam.nl:25
  mysql:
    image: keopx/mysql:5.5
    ports:
      - "3306:3306"
    volumes:
      - ./data/database:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=sirsironenagros
      - MYSQL_DATABASE=kaamelott_db    
      - MYSQL_USER=perceval           
      - MYSQL_PASSWORD=Cestpasfaux
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - "8080:80"
    links:
      - mysql
    environment:
      - PMA_HOST=mysql
```

# Usage

### Start of MAMP

Inside codam_mamp folder (after changing the config inside docker-composte.yml and having docker running) :

```sh
docker-compose up
```

You can add a `-d` option to start it as background task.

If you decide to use this option, you will to run `docker-compose stop` to stop it otherwise a simple `Ctrl-c` will do the job.

### Dev

Ready for battle ?!

You need to place your files `*.php.` inside the folder you defined inside `docker-compose.yml`.

To access the website, use the following url `http://localhost:8008` or `http://zXrXpX.codam.nl:8008` or `http://0.0.0.0:8008` or `http://10.X.X.X:8008` or ...

You also have a container PhpMyAdmin at `http://localhost:8080` ou `http://0.0.0.0:8080` or ...

phpMyAdmin does use MySQL server credential, 

- `MYSQL_ROOT_PASSWORD` - This variable is mandatory and specifies the password that will be set for the root superuser account.

- `MYSQL_USER`, `MYSQL_PASSWORD` - These variables are used in conjunction to create a new user and to set that user's password.


### Problemes ?

- Deal with it

- Read the Manual

- Music always help :

- Gandalf Epic Sax Guy 10 Hours https://www.youtube.com/watch?v=G1IbRujko-A 

- Crab Rave 10 Hours https://www.youtube.com/watch?v=-50NdPawLVY

- Shreksophone 10 hours https://www.youtube.com/watch?v=pxw-5qfJ1dk

# Have Fun!

- Contribution and translation by Thomas from https://github.com/mconnat/101_mamp

- Authors Max,Bodo,Thomas 
