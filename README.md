# Docker + CodeIgniter

## Supported Tags

- `php7.1-apache`, `latest`: Dockerfile
- `php5.6-apache` : Dockerfile.php5.6


## Quick Start

Start a container using the latest image, mapping your local port 80 to the container's port 80:

```shell
$ docker run -p 80:80 --name codeigniter aspendigital/codeigniter:latest
# `CTRL-C` to stop
$ docker rm codeigniter  # Destroys the container
```

> If there is a port conflict, you will receive an error message from the Docker daemon. Try mapping to an open local port (-p 8080:80) or shut down the container or server that is on the desired port.

 - Visit [http://localhost](http://localhost) using your browser.
 - Login to the [backend](http://localhost/backend) with the username `admin` and password `admin`.
 - Hit `CTRL-C` to stop the container. Running a container in the foreground will send log outputs to your terminal.

Run the container in the background by passing the `-d` option:

```shell
$ docker run -p 80:80 --name codeigniter -d aspendigital/codeigniter:latest
$ docker stop codeigniter  # Stops the container. To restart `docker start codeigniter`
$ docker rm codeigniter  # Destroys the container
```

### Environment Variables


Environment variables can be passed to both docker-compose and the container.

 > Database credentials and other sensitive information should not be committed to the repository. Those required settings should be outlined in __.env.example__

 > Passing environment variables via Docker can be problematic in production. A `phpinfo()` call may leak secrets by outputting environment variables.  Consider mounting a `.env` volume or copying it to the container directly.


#### Docker Entrypoint

The following variables trigger actions run by the entrypoint script at runtime.

| Variable | Default | Action |
| -------- | ------- | ------ |
| ENABLE_CRON | false | `true` starts a cron process within the container |
| FWD_REMOTE_IP | false | `true` enables remote IP forwarding from proxy (Apache) |
| PHP_DISPLAY_ERRORS | - | Override value for `display_errors` in docker-oc-php.ini |
| PHP_UPLOAD_MAX_FILESIZE | - | Override value for `upload_max_filesize` in docker-oc-php.ini |
| PHP_POST_MAX_SIZE | - | Override value for `post_max_size` in docker-oc-php.ini |
| PHP_MEMORY_LIMIT | - | Override value for `memory_limit` in docker-oc-php.ini |
