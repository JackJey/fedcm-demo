# fedcm demo

## requirements

- docker
- docker-compose
- mkcert

## start

```sh
$ npm run cert
$ npm start
```

or manually

```sh
# setup mkcert
$ brew install mkcert
$ mkcert -install
# create cert
$ cd proxy
$ ./mkcert.sh
# build and run
$ docker-compose build
$ docker-compose up
```

you can access to the services below.

- https://idp.example
- https://rp.example

## structure

```
|-- .env
|-- docker-compose.yaml # compose every services and nginx
|-- package.json
|-- proxy
|   |-- cert
|   |-- mkcert.sh # run mkcert for each services
|   `-- nginx.conf # config for reverse proxy
`-- services # standalone services
    |-- idp
    |   |-- Dockerfile
    |   |-- package.json
    |   |-- public
    |   |-- server.js
    |   `-- views
    `-- rp
        |-- Dockerfile
        |-- package.json
        |-- public
        |-- server.js
        `-- views
```

nginx forwards every request from browser to backend services.

```
--- nginx:$EXTERNAL_PORT --- services1:$PORT
                         |-- services2:$PORT
                         |-- services3:$PORT
                         `-- ...
```

## adding services

1. implement service under `./services`
2. bind $PORT for `http://`
3. add Dockerfile for service
4. create cert for service via mkcert.sh
5. add `server` section to nginx.conf
6. add `services` section to docker-compose
7. build & run
