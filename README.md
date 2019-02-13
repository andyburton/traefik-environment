# Traefik Developer Environment

This repo contains a Docker script to run Traefik which handles the routing for multiple microservices within an environment e.g. local development. 

Once started it will listen for any containers started on the same network and automagially configure them for you.

To start it up you will need to create a network first (only the first time):

      docker network create microservices

and then spin up the traefik proxy with:

      docker-compose up -d

This will expose a dashboard on http://localhost:8081 and all services on http://localhost:8001/<path to service> or whatever hostname is configured in the traefik labels in the container config.

Container labels should look like this:

      labels:
        - "traefik.frontend.rule=PathPrefix:/api/v1/service"

Or if you want multiple rules like this (path and host are just free text):

      labels:
          - "traefik.path.frontend.rule=PathPrefix:/api/v1/service"
          - "traefik.host.frontend.rule=Host:service.local"     
          
To prevent port conflicts in your environment you should comment out the port mapping for nginx (and talk to the traefik port) and also comment out or change the mysql port.