```sh

docker network create --driver overlay backend && docker network create --driver overlay frontend

docker service create --name result-app --replicas 1 -p 5001:80 --network backend bretfisher/examplevotingapp_result

docker service create --name db --replicas 1 --network backend -e POSTGRES_HOST_AUTH_METHOD=trust --mount type=volume,source=db-data,target=/var/lib/postgresql/data postgres:9.4

docker service create --name worker --network frontend --network backend --replicas 1 bretfisher/examplevotingapp_worker 

docker service create --name redis --network frontend --replicas 1 redis:3.2

docker service create --name vote-app -p 80:80 --network frontend --replicas 2 bretfisher/examplevotingapp_vote



```