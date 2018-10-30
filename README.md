# phpMyAdmin-Docker
how to deploy phpMyAdmin on Docker

- [x] Ale DC

### This Document describes the implementation of phpMyAdmin Docker on your Centos or Ubuntu Server.
### This will connect to other previously created MariaDB or Mysql Dockers.


###### 1 - First download the official image of phpMyAdmin.

```
docker pull phpmyadmin/phpmyadmin
```

###### 2 - Then when we run the docker, we will link (in my case) with MariaDB docker that was previously installed. Maybe you have installed Mysql instead of MariaDB
Keep in mind that both must be in the same network.  (you can verify this by __"docker inspect docker_name"__ and check wich network is used for each container.)

###### 3- Once I know the network used, the name of the database, I proceed to run the phpmyadmi docker, in this opportunity I will use port 34343.

```
docker run --name myadmin -d --link root_mariadb_1:bitnami_suitecrm --network root_default -p 34343:80 -e PMA_HOST=root_mariadb_1 phpmyadmin/phpmyadmin
```

###### 4 - now you can access the graphical tool phpMyAdmin

http://your_domain:34343/




