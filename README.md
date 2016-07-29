# Mapserver in docker container

Use this repo to build a Docker image that serves mapserver CGI via Apache

## 2016-07-29 update

*Debian testing* does not provide libapache2-mod-php5, so created a *jessie* branch where it is available.

## Usage

Firstly, save your [Mapfile(s)](http://mapserver.org/introduction.html#introduction-to-the-mapfile) and data files into a subdirectory, i.e. *maps*. This subdirectory will be mounted into the container as */opt/mapserver/*.

Next, write your Apache configuration file(s) and save it in a subdirectory, i.e.  *sites*. If you are using SetEnv directive to translate *MAP* parameter value e.g. *mymap* in CGI query string to absolute path of the Mapfile, the Apache directive would look like this:

```
SetEnv mymap /opt/mapserver/mymap.map
```

The easiest way to build and run the container is by using [docker-compose](https://docs.docker.com/compose/). The sample docker-compose.yml given below assumes that you have used the same subdirectory names otherwise modify them accordingly.

```
mapserver:
  image: cheewai/mapserver
  #container_name: mapserver
  ports:
    - "80:80"
  volumes:
    - ./sites:/etc/apache2/sites-enabled:ro
    - ./maps:/opt/mapserver:ro
    - ./logs:/var/log/apache2
```

