# Repository Versioning

This README covers how to update and manage release version tags in repositories used by our Earthfile located in the `stoikio/product` repository.

## Updating Release Versions

### Using Git CLI

1. Ensure a clean and updated working directory.
2. Run `git tag -a vx.x.x -m "Release vx.x.x"` to create a new version tag.
3. Push the tag with `git push origin vx.x.x`.

### Using GitHub

1. Navigate to the repository's main page on GitHub.
2. Click the "Releases" tab.
3. Click "Create a new release".
4. Enter the version tag (e.g., v1.2.3) in the "Choose a tag" field.
5. Add a title and description, and click "Publish release".

## Updating the Earthfile

1. Renovate will create a PR on the `stoikio/product` repository to update the corresponding version tag in the `.env.static` file
2. Merge this PR
3. The next build will automatically pull the latest version of this repository

# Bull Board

Docker image for [bull-board]. Allow you to monitor your bull queue without any coding!

Supports both: bull and bullmq. bull-board version v3.2.6

### Quick start with Docker

```
docker run -p 3000:3000 deadly0/bull-board
```

will run bull-board interface on `localhost:3000` and connect to your redis instance on `localhost:6379` without password.

To configurate redis see "Environment variables" section.

### Quick start with docker-compose

```yaml
version: "3.5"

services:
    bullboard:
        container_name: bullboard
        image: deadly0/bull-board
        restart: always
        ports:
            - 3000:3000
```

will run bull-board interface on `localhost:3000` and connect to your redis instance on `localhost:6379` without password.

see "Example with docker-compose" section for example with env parameters

### Environment variables

-   `REDIS_HOSTNAME` - host to connect to redis (localhost by default)
-   `REDIS_PORT` - redis port (6379 by default)
-   `REDIS_DB` - redis db to use ('0' by default)
-   `REDIS_USE_TLS` - enable TLS true or false (false by default)
-   `REDIS_PASSWORD` - password to connect to redis (no password by default)
-   `BULL_PREFIX` - prefix to your bull queue name (bull by default)
-   `BULL_VERSION` - version of bull lib to use 'BULLMQ' or 'BULL' ('BULLMQ' by default)
-   `PROXY_PATH` - proxyPath for bull board, e.g. https://<server_name>/my-base-path/queues [docs] ('' by default)
-   `USER_LOGIN` - login to restrict access to bull-board interface (disabled by default)
-   `USER_PASSWORD` - password to restrict access to bull-board interface (disabled by default)

### Restrict access with login and password

To restrict access to bull-board use `USER_LOGIN` and `USER_PASSWORD` env vars.
Only when both `USER_LOGIN` and `USER_PASSWORD` specified, access will be restricted with login/password

### Example with docker-compose

```yaml
version: "3.5"

services:
    redis:
        container_name: redis
        image: redis:5.0-alpine
        restart: always
        ports:
            - 6379:6379
        volumes:
            - redis_db_data:/data

    bullboard:
        container_name: bullboard
        image: deadly0/bull-board
        restart: always
        ports:
            - 3000:3000
        environment:
            REDIS_HOSTNAME: redis
            REDIS_PORT: 6379
            REDIS_PASSWORD: example-password
            REDIS_USE_TLS: "false"
            BULL_PREFIX: bull
        depends_on:
            - redis

volumes:
    redis_db_data:
        external: false
```

[bull-board]: https://github.com/vcapretz/bull-board
[bull-board]: https://github.com/felixmosh/bull-board#hosting-router-on-a-sub-path
