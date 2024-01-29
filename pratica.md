Etapas:

#iniciar zookeeper
./zookeeper-server-start.sh ../config/zookeeper.properties

#iniciar broker
./kafka-server-start.sh ../config/server.properties


criar tópico: 
./kafka-topics.sh --bootstrap-server 127.0.0.1:9092 --topic apachelog --create --partitions 3 --replication-factor 1

./kafka-topics.sh --bootstrap-server 127.0.0.1:9092 --list


criar consumidor: 
./kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic apachelog 


jeniffer@DM-004425:~$ sudo systemctl status apache2
cd /var/log/apache2
cat access.log


jeniffer@DM-004425:~$ python3 connectorapache.py 
