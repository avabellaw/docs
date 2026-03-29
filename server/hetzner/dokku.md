## Create new

dokku apps:create *
dokku git:set * deploy-branch main
dokku git:sync --build [app] *


## Env vars

dokku config:set - -


## "projects"

### Add buildpacks

https://github.com/timanovsky/subdir-heroku-buildpack

## setup ssl (https)

tar csf .crt .key
dokku certs:add myrecipe < ./cert-key.tar


## Postgres

### Export from postgres 

pg_dump -Fc --no-acl --no-owner [pg url] > data.dump

### import data

dokku postgres:import APP < data.dump

## Create and link to service

```
dokku postgres:create postgres
dokku postgres:link postgres app
```

## Specify sub directories

1. If this is the only buildpack, then add the config var ```BUILDPACK_URL``` to the dokku app and then set it to ```https://github.com/heroku/heroku-buildpack-python.git```

    Add '.buildpacks' at root of repo to add multiple buildpacks. Include:
    ```https://github.com/heroku/heroku-buildpack-python.git```


2. Set the config var ```PROJECT_PATH``` to the name of the folder you want to specify.

## Enable ssl

zip the .crt and .key files for ssl into a tar file and add to app

``` dokku certs:add [appname] < ./dokku-cert-key.tar```

## Add from image 

EXAMPLE_APP_NAME=gotify

```dokku git:from-image gotify gotify/server:latest```