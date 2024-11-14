# Creating vm's with multipass

Launching three ubuntu instances and install docker:
``` sh
for ($i = 1; $i -le 3; $i++) {
    multipass launch --name node-$i
    multipass exec node-$i -- sh -c "curl -sSL https://get.docker.com/ | sh"
}
multipass list
```

Asynchronous initialization:
``` sh
for ($i = 1; $i -le 5; $i++) {
    Start-Job -ScriptBlock {
        multipass launch --name instance-$using:i --cpus 2 --mem 2G --disk 10G
    }
}
multipass list
```

# Setting up swarm
``` sh
multipass shell node-1
sudo su -
docker swarm init

multipass shell node-$i
sudo docker swarm join --token adsf...
```

``` sh
docker network create --driver overlay mydrupal
docker service create --name psql --network mydrupal -e POSTGRES_PASSWORD=mypass postgres
docker service ls
docker service ps psql

docker service create --name drupal --network mydrupal -p 80:80 drupal
docker service ls
```

``` sh
docker inspect node-x
```

Thnaks to the routing mesh Drupal now responds on each ip address from different nodes, even though it is running just on one node. Thanks to this we don't need to worry about where the service is running as this may change and swarm routing will take care of sending the traffic to the correct node.

Regading the configuration of Drupal, to establish a connection between Drupal and PostgresSQL we will use the psql service name as the address. With overlay network everything acts like it is on the same subnet

