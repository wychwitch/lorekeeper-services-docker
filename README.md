# Lorekeeper Services Docker

This is an unnoficial docker setup for running [Lorekeeper](https://github.com/lk-arpg/lorekeeper)'s services (Apache, Mariadb, & Adminer) via docker.

This was mainly created so I didn't have to deal with setting up both a local and production environment and deal with their own individual quirks. This *should* make Lorekeeper services more reproducible and consistent across servers!

**Warning: I am not an expert on web development nor Docker best practices**. Use at your own risk. If any suggested or changes can be made, please feel free to open a PR or an issue! 

Also, if you are brand new to web development, server managment, or laravel development at all, I actually wouldn't recommend  you follow this guide and instead try and get Lorekeeper up and running manually first! Only then would I recommend using this docker setup, as involving docker is another layer of complexity that might make things confusing for you down the road. However, I made this in such a way it *should* be easy to move to another setup with relative ease if that does happen.

## How to use this repo
First, clone the repo somewhere:

```bash
git clone https://github.com/wychwitch/lorekeeper-services-docker.git 
```

Navigate to where you cloned it and copy & edit the `.env.example` file variables as needed, taking care that the database info matches the info in your lorekeeper's .env file!

```bash
cp .env.example .env
```

Launch and build the services using docker compose

```bash
docker compose up --build
```

If everything goes well and the initial setup finishes, then hit ctrl-c to close it and run it in the bg:

```bash
docker compose up -d
```

Your database's data will be in the `./data/database` folder!

## Adminer

Included alongside the database and web server is a small program called [Adminer](https://www.adminer.org/), which is very similar to phpmyadmin. If you navigate to the site (default `localhost:8181`) You sign in using the `DB_USERNAME`, `DB_PASSWORD`, and `DB_DATABASE` variables from before! The host should automatically have `db` in its field but that's totally fine, It'll work!

If you do not want Adminer because you already have phpmyadmin or another database management solution, simply delete the relevant lines from the docker-compose.yml!

## Running Compose & Laravel Scripts

The easiest way to do this is to log into the running docker container using this command in the services repo directory:

```bash
docker compose exec --user lorekeeper web bash
```
You can log out of the container by running `exit`

However you can also remotely execute commands without needing to log in first by using docker exec: 

```bash
docker exec -it lorekeeper-services-frontend-1 sh -c "php artisan migrate"
```

and if that's too much to copy and paste each time, I made two small two-line scripts called lk-exec.sh and lk-login.sh in the repo's directory that you can alias, move to your bin, what have-you  

lk-exec

```bash
#!/bin/bash
docker exec -it lorekeeper-services-frontend-1 sh -c "$*"
```

lk-login

```bash
#!/bin/bash
docker compose exec --user lorekeeper web bash
```

## Credits
The docker-compose file & the Dockerfile were based on [this incredibly helpful guide](https://inovector.com/blog/minimal-configuration-docker-to-run-laravel-application). 
