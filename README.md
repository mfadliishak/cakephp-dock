# docker-cakephp3-template

By using Docker-Compose, you can build CakePHP3 environment easily.

# Environment

* Docker 18.06.1-ce
* Docker Compose 1.22.0
* Docker Machine 0.15.0

## Initialization (Build CakePHP3 Project)
Since CakePHP3 has an execution environment for each project, so create a project. To create another project, follow the same procedure.

### Create Docker Images
It will take about two or three minutes.

```
$ docker-compose build
```

### Run Containers 

```
$ docker-compose up -d
```

### Check containers ###

```
$ docker ps
```

### Enter PHP-FPM Container

```
$ docker exec -it docker-cakephp3-template_phpfpm_1 /bin/sh
```


### Get Composer Installer

```
/var/www/html # curl -s https://getcomposer.org/installer | php
```

### Pull Project into app folder

`app` folder should be inside data/htdocs which is ignored by git.
Pull cakephp project or create cakephp project into this folder

```
/var/www/html # git clone https://digitaleaf.backlog.jp/git/ATTENDANCE/attendance.web.git app
/var/www/html # cd app/source && php ../../composer.phar install
```

You can change this behavior by modifying `PRJ:` value in `docker-compose.yml` file

```
host:
    build: ./data/htdocs
    environment:
      TZ: "Asia/Tokyo"
      PRJ: "app" # path of the cakephp project
```

### Exit Container

```
/var/www/html # exit
```

### Fix Configuration
Update `data/htdocs/sample/config/app.php` at line 251

```data/htdocs/sample/config/app.php
-   'host' => 'localhost',
+   'host' => 'mysql',
```

## Build Up

```
$ docker-compose up -d
```

You can check at http://localhost:8765

<img width="1602" alt="スクリーンショット 2018-09-18 3.26.26.png" src="https://qiita-image-store.s3.amazonaws.com/0/291512/b748a76d-e2fa-e56b-0cf1-f938b6194a30.png">

## Remove Containers

```
$ docker-compose down 
```

## Other topics

※We can add other cakephp project into data/htdocs/app and change `PRJ:` as instructed above accordingly.

※Other cakephp project may encounter mysql error `SQLSTATE[HY000] [2002] No such file or directory`.
This can be solved by modifying cakephp's `config/app.php` under database host should change the IP address

```
'Datasources' => [
        'default' => [
            'className' => 'Cake\Database\Connection',
            'driver' => 'Cake\Database\Driver\Mysql',
            'persistent' => false,
            'host' => 'localhost', #here need to change
```

What IP address to specify here, must access mysql server as follows:

```
$ docker exec -it docker-cakephp3-template_mysql_1  /bin/bash
$ cat /etc/hosts
```

it will shows something like this:

```
127.0.0.1   localhost
::1 localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.21.0.2  371725c16aa6
```

`371725c16aa6` is DB container ID, so we use `172.21.0.2` to the `app.php` for above:

```
'host' => '172.21.0.2',
```
