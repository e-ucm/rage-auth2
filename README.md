# RAGE Authentication and Authorization

This repository contains scripts to launch and manage the RAGE Authentication and Authorization server-side asset and its dependencies (Redis and Mongo).

This server is part of the [RAGE](http://rageproject.eu/) EU H2020 project 
(Realizing an Applied Game Ecosystem), and provides authentication and authorization for server-side assets. It is a dependency [rage-analytics environment](https://github.com/e-ucm/rage-analytics/), and we recommend it be used by all other server-side assets.

Note that the rage-analytics environment *already includes* ; if you are already launching the analytics environment, you should not use this script.

## Hardware and Software Requirements

We rely on [docker](https://docs.docker.com/installation/) to modularize and simplify deployment; and on [docker-compose](https://docs.docker.com/compose/) to manage and orchestrate (or, dare I say, _compose_) those containers.

You will need docker v1.9 or greater and docker-compose v1.5 or greater installed. Mongo requires around 3 Gb free disk to run (after installing all images); so plan to have at least 4 Gb free disk space.

## Simple usage

0. Open a shell in a recent linux (we use Ubuntu 14.04+). You must be root (`sudo su -`) unless you already have `docker` running and a compatible version of `docker-compose` installed 
1. Download the launch script: `wget https://raw.githubusercontent.com/e-ucm/rage-analytics/master/rage-a2.sh`
2. Mark the script as executable, and launch it: `chmod +x rage-a2.sh && ./rage-a2.sh launch` (note that it requires `bash` to run). Besides `launch`, the scripts accepts several other commands - use `./rage-a2.sh --help` to see their names and descriptions.

... and type `docker-compose ps` to check that everything has been launched. Expected output:

```
           Name                         Command               State                    Ports                  
-------------------------------------------------------------------------------------------------------------
rageanalytics_a2_1           npm run docker-start             Up       0.0.0.0:3000->3000/tcp                 
rageanalytics_mongo_1        /entrypoint.sh mongod            Up       27017/tcp                              
rageanalytics_redis_1        /entrypoint.sh redis-server      Up       6379/tcp                               
```

The following services will be launched:
* `a2` at `http://localhost:3000`: running [Authentication&Authorization](https://github.com/e-ucm/a2) server. Allows registering server-side applications (such as the `rage-analytics-backend`) 

Exposed ports can be easily altered by modifying `docker-compose.yml` (eg.: changing the `a2` port to `3000:4000`) would expose `a2` in `4000` instead of the default `3000`.

## Troubleshooting

The `report` command generates a text file with information that can help us diagnose any problems during installation or execution. It does not include any personally-identifiable [information](https://github.com/e-ucm/rage-analytics/blob/master/rage-a2.sh#L127) (in particular, neither your machine's public IP  nor your username is included; although we do want to know if you are running it as root or using a `docker` group).

When you have a problem,

- run `./rage-a2.sh report` (_before_ stopping the services)
- open an issue on our [issues page](https://github.com/e-ucm/rage-a2/pulls) (if you register as a user on github, you will be e-mailed as soon as we comment on the issue)
- append the report to your new issue, and described the problem and the steps to reproduce it as accurately as possible. We will get back to you as soon as we can.
