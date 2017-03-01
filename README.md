# Hub for multi PMIS instances

Before running the service create the network with the following command:

    $ docker network create hub_net

Then start the service with:

    $ docker-compose up --build -d

Or to check new images and update and build with:

    $ docker-compose pull
    $ docker-compose build --pull
    $ docker-compose up -d