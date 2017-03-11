# Sample application

This is just a really simple web page to test with the hub,
follow below to run it.

Create a file named `sample.conf` inside the `conf.d` directory,
that is located just under the root directory (NOT inside the `hub` folder),
and put the following content in it:

    server {
        listen 80;
        server_name 192.168.99.100;
        
        location / {
            set $upstream_webapp sample;
            proxy_pass http://$upstream_webapp;
            
            include conf.d/proxy-header.include;
        }
    }

Use docker-compose to start the web application:

    $ docker-compose up -d

If it works you should see the Nginx welcome page.