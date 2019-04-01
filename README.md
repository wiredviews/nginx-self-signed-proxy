Nginx reverse proxy with self-signed SSL cert
============

1. Build the nginx container with a self-signed ssl certificate with a lifetime of ten years.

```bash
cd nginx
docker build -t nginx:ten-year-cert .
```

This will take a base Alpine container, add openssl and generate a ten year certificate for localhost. It will also copy any ```*.conf``` file in the ```nginx/conf.d``` folder to ```/etc/nginx/conf.d``` in the image.

It also makes a few assumptions; one being that the proxied service container will have an HTTP listener on  port 9999.  

2. Adjust the ```proxied-service``` declaration for the service you want to proxy.  In this case, this is PlantUML. Make sure your service is listening on port 9999.

```yaml
proxied-service:
    environment:
      JAVA_OPTIONS: "-Djetty.port=9999"
    image: plantuml/plantuml-server:jetty
    container_name: proxied-server
    expose:
      - 9999
```

3.  Start up the composition via docker-compose:

```bash
$ docker-compose -d up

Creating network "ssl-proxy_default" with the default driver
Creating proxied-server ... done
Creating nginx-proxy    ... done
```

At this point, your service should be available at https://<<machine name>>/

You'll have to add a security exception in your browser the first time you access it.

Happy Proxying!