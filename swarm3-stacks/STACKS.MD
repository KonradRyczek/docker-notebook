# Swarm Stacks

## Setting up a stack
Docker-compose for docker on the host serves as a great tool to more simply bootstrap a few containers with their configs, networks and stuff... Similar to that we have comopose for swarm that will act similarly but for services, letting us also bootstrap volumes, networks, secrets and others in a more convenient way.

It takes the yaml file and ignores 'build' information, and creates what's called a stack. One stack corresponds to one swarm

Having the compose file 
` docker stack deploy -c compose.yml app `
It created the tasks in the scheduler

We can check the status of the services with
` docker stack services app  `
` docker stack ps app `

To update changes in a stack we must use the stack deploy command. Docker will recognize the needed changes and update accordingly.
` docker stack deploy -c compose.yml app `

