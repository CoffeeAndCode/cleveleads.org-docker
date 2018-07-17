# GiveCamp Docker Setup

## Getting Started

You will need:

- Docker installed on your computer
- a zip file containing all of the WordPress / platform source code
- a sql file dump from the production database

You can then perform the following steps:

1. Unzip the website code and put it in a folder called `src` in the rool of this project.
2. Unzip the database and put the sql file in the root of this project called `backup.sql`.
3. Run `docker-compose up` to install the containers and kick everything off.
4. Run `bin/setup` ONCE to import and clean up the database for local work.
You do not need to run this command each time.

You can then visit http://localhost:8080 to view the website.

On subsequent starts, you only need to run `docker-compose up` and then visit
http://localhost:8080.

## The Platform

Add documentation here on how to access the local platform install as
well as the setup steps necessary to work from a local install.

## WordPress CLI

You can run WP Cli commands after the WordPress site is running by running
docker commands similar to the following:

```
# Get info about CLI command
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli help

# List users
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli user list

# Change user's password
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli user update 1 --user_pass=givecamp

# Export database inside Docker to a file
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli db export - | tee ~/Desktop/export.sql

# Reset existing database inside Docker
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli db reset

# Import existing production database backup
cat ../backup.sql | docker run -i --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli db import -

# Update imported database to have correct paths
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli search-replace '/srv/users/clcuser/apps/cleveleads/public/wp-content' '/var/www/html/wp-content' --skip-columns=guid --all-tables

docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli search-replace 'https://www.cleveleads.org' 'http://localhost:8080' --skip-columns=guid --all-tables

docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli search-replace 'http://www.cleveleads.org' 'http://localhost:8080' --skip-columns=guid --all-tables

# Check that database is functioning properly
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli db check

# List wp-config.php values
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli config list

# List WordPress options
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli option list

# Update WordPress options to work with local install
docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli option update home 'http://localhost:8080'

docker run -it --rm --volumes-from cleveleadsorg-docker_wordpress_1 --network container:cleveleadsorg-docker_wordpress_1 wordpress:cli option update siteurl 'http://localhost:8080'
```

#### Resources

- https://guides.wp-bullet.com/advanced-wordpress-database-https-search-replace-with-wp-cli/
