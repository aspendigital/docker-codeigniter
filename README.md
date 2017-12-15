# Docker + CodeIgniter 1.7.3

This image serves as a starting point for legacy CodeIgniter projects.

> The [CodeIgniter User Guide](https://github.com/aspendigital/docker-codeigniter/tree/master/CodeIgniter_1.7.3/user_guide) is served up by default if nothing has been copied or mounted to `/var/www/html`.

## Supported Tags

- `php5.6-apache`, `latest`: [Dockerfile](https://github.com/aspendigital/docker-codeigniter/blob/master/Dockerfile)
- `php7.1-apache` : [Dockerfile.php7.1](https://github.com/aspendigital/docker-codeigniter/blob/master/Dockerfile.php7.1)


## Quick Start

Start a container using the latest image, mapping your local port 80 to the container's port 80:

```shell
$ docker run -p 80:80 --name codeigniter aspendigital/codeigniter:latest
# `CTRL-C` to stop
$ docker rm codeigniter  # Destroys the container
```

> If there is a port conflict, you will receive an error message from the Docker daemon. Try mapping to an open local port (-p 8080:80) or shut down the container or server that is on the desired port.

 - Visit [http://localhost](http://localhost) using your browser.
  - Hit `CTRL-C` to stop the container. Running a container in the foreground will send log outputs to your terminal.

Run the container in the background by passing the `-d` option:

```shell
$ docker run -p 80:80 --name codeigniter -d aspendigital/codeigniter:latest
$ docker stop codeigniter  # Stops the container. To restart `docker start codeigniter`
$ docker rm codeigniter  # Destroys the container
```

## Working with Local Files

Using Docker volumes, you can mount local files inside a container.

> The CodeIgniter system folder resides in `/var/www/codeigniter`. You may need to update the `$system_folder` variable in your project's `include/ci_include.php` file.

The container uses the working directory `/var/www/html` for the web server document root. You can introduce your local project code with bind-mounted volumes:

```shell
$ docker run -p 80:80 --rm \
  -v $(pwd):/var/www/html \
  aspendigital/codeigniter:latest
```

Save yourself some keyboards strokes, utilize [docker-compose](https://docs.docker.com/compose/overview/) by introducing a `docker-compose.yml` file to your project folder:


```yml
# docker-compose.yml
version: '2.2'
services:
  web:
    image: aspendigital/codeigniter:latest
    ports:
      - 80:80
    volumes:
      - $PWD:/var/www/html
```
With the above example saved in working directory, run:

```shell
$ docker-compose up -d # start services defined in `docker-compose.yml` in the background
$ docker-compose down # stop and destroy
```

### Environment Variables

Environment variables can be passed to docker-compose.

The following variables trigger actions run by the entrypoint script at runtime.

| Variable | Default | Action |
| -------- | ------- | ------ |
| ENABLE_CRON | false | `true` starts a cron process within the container |
| FWD_REMOTE_IP | false | `true` enables remote IP forwarding from proxy (Apache) |
| PHP_DISPLAY_ERRORS | off | Override value for `display_errors` in docker-ci-php.ini |
| PHP_POST_MAX_SIZE | 32M | Override value for `post_max_size` in docker-ci-php.ini |
| PHP_MEMORY_LIMIT | 128M | Override value for `memory_limit` in docker-ci-php.ini |
| PHP_UPLOAD_MAX_FILESIZE | 32M | Override value for `upload_max_filesize` in docker-ci-php.ini |
| TZ | UTC | Override value for `data.timezone` in docker-ci-php.ini |
