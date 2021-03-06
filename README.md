# cytube-docker
Docker compose for sync by calzoneman

https://github.com/calzoneman/sync

## Disclaimer

This image is not officially supported by the main CyTube developers.  It is maintained by third party contributors.  Feel free to submit pull requests if you'd like to fix or improve the image.

The image cytube/cytube:latest is automatically built from the master branch at GitHub. That branch is intended to stay stable, and will not be updated until the changes have been tested in another branch/image. We suggest using the latest tag in production. The develop image (cytube/cytube:develop) is automatically built from the develop branch at GitHub. We advise against using the develop tag in production.

## How to get started

To run this application first you must clone the repo using ```git clone --recursive https://github.com/CyTube/cytube-docker.git```

Next you will need to run copy_config.sh in order to automatically copy the latest versions of the config from the sync folder. You may also choose to do this manually. You will need the conf/example and a config.yaml files so that they can be mounted into the container. The script is there to make this easier for you by downloading the necessary files.

After you have the files, open the config.yaml file and edit the information in the config to meet your specific needs.
calzoneman has thoroughly commented the file to give you a good idea of what needs to be edited.

After editing your config.yaml file you will need to edit the docker-compose.yml to match what you have entered in the config.yaml file. It is commented to give you an idea of what needs to be edited.

When you have finished the previous steps start the containers by running ```docker-compose up -d``` this will cause docker-compose to build the Dockerfile and start the sync and mariadb/mysql containers.

## Nginx Reverse Proxy Config

Here is an example config for using Nginx to reverse proxy sync using https on port 8443.

```
server {

    listen 443 ssl http2;
    server_name sync.example.com;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;


        location / {
                proxy_pass https://sync:8443;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header Host $http_host;
                   }
}
```
