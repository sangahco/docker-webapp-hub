# Hub for PMIS instances

Thss web server is used as a controller for several PMIS application running on the same server.

## Configuration

Under the folder `hub/conf.d` there are two examples of configuration for one PMIS application.

**backed1.conf.sample** - This is used for *SSL/TLS* connection with redirection from port *80* to *443*.

**backed2.conf.sample** - This is the simple *HTTP* connection.

Make a copy of one of these example, you can rename it with the name of the application.
Inside you need to change the *dns* (the name after the directive `server_name`) 
and the upstream server name (the name after the directive set `$upstream_webapp`).

The upstream name is the value you gave to the variable [`HUB_INSTANCE`](https://github.com/sangahco/docker-pmis-app/blob/master/README.md "PMIS deploy guide")

---

Before running the service create the network with the following command:

    $ docker network create hub_net

## Run using `docker-auto` script

This is the recommended way to run this service, start the service with the following command:

    $ ./docker-auto --prod up

or if you are editing and testing the service with:

    $ ./docker-auto --dev up

See the help for more commands:

    $ ./docker-auto --help


## Run with `docker-compose`


Then start the service with:

    $ docker-compose up --build -d

Or to check new images and update and build with:

    $ docker-compose pull
    $ docker-compose build --pull
    $ docker-compose up -d