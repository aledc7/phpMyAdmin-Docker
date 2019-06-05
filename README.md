# phpMyAdmin-Docker
how to deploy phpMyAdmin on Docker

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

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

_______________________________________________________________________________________________________________________
### Add Multiple Hosts in phpMyAdmin

You may want to access different database engines with the same instance of phpMyAdmin.
This is possible by editing the phpmyadmin configuration file, located inside the phpMyAdmin docker.

First of all it is necessary to access the bash of the docker, we must consider that the phpmyadmin docker __does not have bash incorporated__, therefore the way to access will be different from the traditional one (docker exec -u 0 -it / bin / bash) this form It will not work with this particular docker. Therefore we must access in this other way usin **busybox ash**:

```
docker exec -it docker_name busybox ash
```
once inside the phpmyadmin docker, we will find the main configuration file in the following location:

```
/etc/phpmyadmin/config.inc.php
```
Finally, it only remains to add the following fragment of code, replacing the data 'verbose', 'host', and other data that are different from yours

```
/* Agregado por AleDC - Esto permite multiple Host */

$i++;
$cfg['Servers'][$i]['verbose'] = 'mautic_mauticdb_1';
$cfg['Servers'][$i]['host'] = 'mautic_mauticdb_1';
$cfg['Servers'][$i]['port'] = '3306';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysqli';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['AllowNoPassword'] = false;

# Esta linea de abajo permite borrar las bases de datos, por defecto a veces viene deshabilitado

$cfg['AllowUserDropDatabase'] = false;

```

In case it does not work, verify that both containers are in the same docker network.


1- Inspect the dockers and verify which network are assigned, find the parameter "Networks": {
```
docker inspect docker_name
```
2- add the same network to the docker that does not have it
```
docker network connect docker_network_name docker_name_to_asign_the_network
in my case:
docker network connect root_default mautic_mauticdb_1
```
that is all

- [x] Ale DC



