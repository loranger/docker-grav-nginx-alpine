# Lightweight PHP-FPM & Nginx Docker Image for Grav CMS

A Docker for quickly getting [Grav](https://getgrav.org/) CMS running on PHP 7.1 and Nginx under Alpine.

The Automated Docker Build of this repo is located [here](https://hub.docker.com/r/tonyisworking/grav-php7/). It should working and ready to go, if you have issues feel free to ask.

## Overview

The web root folder is located at `/var/www/grav`

Grav is already preloaded into the web root folder, so you can just start the container and you're up and running. 

If you want to mount your own grav installation feel free to do so by overriding the web root location. 

If you want to load your own `user` folder, then mount your files into `/var/www/grav/user`. An example of this is in the sample docker compose file.

If the container is loaded with `INSTALL_CMS=true` then grav will be re-installed upon initialization into the web root folder. Useful if you externally mounted a directory at that location. 

You can turn on php opcache with `BUILD_ENV=prod`

I've also loaded a copy of the [html5boilerplate](https://html5boilerplate.com/) 404 page as the 404 page for nginx. 

## Docker-Compose Sample

I'm making use of [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) to easily handle routing among multiple containers. Feel free to not use it.

```
version: '2'
services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    ports:
    - '80:80'
    - '443:443'
    volumes:
    - '/var/run/docker.sock:/tmp/docker.sock:ro'

  grav:
    image: tonyisworking/grav-php7
    container_name: grav
    ports:
    - '8001:80'
    volumes:
    - {path_to_grav_user_folder}:/var/www/grav/user'
    environment: 
    - 'VIRTUAL_HOST=grav.dev'
    - 'VIRTUAL_PORT=80'
    - 'INSTALL_CMS=TRUE'
    - 'BUILD_ENV=prod'

```
### Steps to get going:

- Put the above into a `docker-compose.yml` file. 
- Replace `{path_to_grav_user_folder}` with your user folder path or delete this line as well as the `volumes:` line
- Add `127.0.0.1 grav.dev` to your hosts file.
- Open up your terminal to your where you put `docker-compose.yml` and run `docker-compose up -d`
- Open your browser and navigate to `grav.dev`
- You should see Grav saying it is up and running!


## What's inside this container:
- php7
- php-fpm7
- composer
- nginx
- alpine

## License
[GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html)

This docker was based off of [docker-wordpress](https://github.com/devgeniem/docker-wordpress) settings.

