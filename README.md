# CHIRPSTACK NW SERVER

CHIRPSTAN NW SERVER ROADMAP:

-Creación de Maquina virtual AWS con ubuntu 18.04 LTS

1GB RAM

1 - Actualización de paquetes:

 sudo apt-get update \n
 sudo apt-get upgrade \n

Habilitar SSH:

https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/
sudo apt update
sudo apt install openssh-server
sudo systemctl status ssh
sudo ufw allow ssh

-Instalación de Postgre:

sudo apt-get install postgresql

-Instalación de Redis:
 Redis is an open source (BSD licensed), in-memory data structure store, used as a database,
 cache, and message broker. Redis provides data structures such as strings, hashes, lists, 
 sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes, and streams. 
 Redis has built-in replication, Lua scripting, LRU eviction, transactions, and different levels 
 of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning 
 with Redis Cluster.
 
 Redis database
ChirpStack Network Server uses Redis for storing device-session data and non-persistent data like distributed locks, deduplication sets and meta-data.

Notes: * At least Redis 2.6.0 is required. * Flushing the Redis database means all devices have to rejoin (OTAA) the network.
 
 -Instalación de MQTT Broker Debian / Ubuntu:
 
ChirpStack Network Server makes use of MQTT for publishing and receiving application payloads. Mosquitto is a popular open-source MQTT server, 
but any MQTT broker implementing MQTT 3.1.1 should work. In case you install Mosquitto, make sure you install a recent version.

sudo apt install mosquitto

RUTA: /etc/mosquitto

See http://mosquitto.org/ for more information.

mosquitto - running the Mosquitto broker
mosquitto.conf - the Mosquitto broker configuration file
mosquitto_passwd - command line utility for generating Mosquitto password files
mosquitto_pub - command line utility for publishing messages to a broker
mosquitto_rr - command line utility for simple request/response with a broker
mosquitto_sub - command line utility for subscribing to topics on a broker
mosquitto-tls - brief cheat sheet for creating x509 certificates
mqtt - description of MQTT features

- INSTALACIÓN DE CHIRPSTACK LORA NW SERVER


Debian / Ubuntu repository
As all packages are signed using a PGP key, you first need to import this key:


sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1CE2AFD36DBCCA00
To add the ChirpStack Network Server repository to your system:


sudo echo "deb https://artifacts.chirpstack.io/packages/3.x/deb stable main" | sudo tee /etc/apt/sources.list.d/chirpstack.list
sudo apt update

sudo apt install chirpstack-network-server



The configuration file is located at:
 /etc/chirpstack-network-server/chirpstack-network-server.toml

Some helpful commands for chirpstack-network-server:

Start:
 $ sudo systemctl start chirpstack-network-server
Restart:
 $ sudo systemctl restart chirpstack-network-server
Stop:
 $ sudo systemctl stop chirpstack-network-server
Display logs:
 $ sudo journalctl -f -n 100 -u chirpstack-network-server
---------------------------------------------------------------------------------




Created symlink /etc/systemd/system/loraserver.service → /lib/systemd/system/chirpstack-network-server.service.
Created symlink /etc/systemd/system/multi-user.target.wants/chirpstack-network-server.service → /lib/systemd/system/chirpstack-network-server.service.
ubuntu@ip-172-31-86-186:~$d


