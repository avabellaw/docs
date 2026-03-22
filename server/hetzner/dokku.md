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

