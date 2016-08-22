### network containers

#### name container 

* docker run -d -P --name web training/webapp python app.py

### list networks

* $ docker network ls

NETWORK ID          NAME                DRIVER
18a2866682b8        none                null                
c288470c46f6        host                host                
7b369448dccb        bridge              bridge  

* $ docker network inspect bridge

* $ docker network disconnect bridge networktest -- disconnect container from network

** Networks are natural ways to isolate containers from other containers or other networks. 

## create network 

* docker network create -d bridge my-bridge-network

`-d` flag tell docker to use the bridge driver for the new network.

### add containers to a network

* $ docker run -d --network=my-bridge-network --name db training/postgres
* $ docker run -d --name web training/webapp python app.py

these two are running in different network, can not access each other.

the network info:

➜  ~ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web
172.17.0.2
➜  ~ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' db
172.18.0.2


* docker exec -it db bash
* ping 172.17.0.2 

#### connect web to my-bridge-network, then it's accessable.
* $ docker network connect my-bridge-network web

$ docker exec -it db bash
ping web


➜  ~ docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web
172.17.0.2172.18.0.3
