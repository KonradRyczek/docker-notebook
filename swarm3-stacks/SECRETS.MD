## Using sercrets in swarm services
Ideal for usernames, passwords, tls certificates and keys, ssh keys ... It supports generic strings or binaries up to 500kb.

Creting from filename (drawback file stored on the disk )
` docker secret create name sercretfile `
Passing through the command line
` echo "the secret" | secret create name - `

Once we store the secret on docker, the only thing that will have acess to it are the containers and services we assigned the sercret to.

` docker service create --name psql --secret psql_user --secret psql_pass -e POSTGRES_PASSWORD_FILE=/run/secrets/psql_pass -e POSTGRES_USER_FILE=/run/secrets/psql_user postgres`

## Using secrets in swarm stacks
Similarly to the swarm we either create the secrets as a file of in docker. Then we specify in its special keyword `secrets` that we will be using some secret. And in each service that will need our secret we specify it, again using the `secrets` keyword.

Secrets under a service can be defined with permissions, users and other security options. (f.e. non root app).

``` yml
services:
    psql:
        image: postgres
        secrets:
            - psql_user
            - psql_password
        environment:
            POSTGRES_PASSWORD_FILE: /run/secrets/psql_password
            POSTGRES_USER_FILE: /run/secrets/psql_user

secrets:
    psql_user:
        file: ./psql_user.txt
    psql_password:
        file: ./psql_password.txt
```

Removing the stack will clean-up the created secrets.