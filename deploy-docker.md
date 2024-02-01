# Deploy with `Docker`

## Web (Angular)

update file `package.json`

```json
"scripts": {
    "build": "ng build --configuration production --base-href=./ --output-path=dist/web",
},
```

create `Dockerfile`

```dockerfile
FROM node:20-alpine
LABEL maintainer="Satit Rianpit <rianpit@gmail.com>"
WORKDIR /home/web
COPY . .
RUN npm i
RUN npm run build
RUN npm i express
CMD ["node", "server.js"]
EXPOSE 80
```

create `server.js`

```js
var express = require('express');
var path = require('path');
var app = express();

app.use(express.static(path.join(__dirname, './dist/web/browser/')));

app.get('*', function (req, res) {
	res.sendFile(path.join(__dirname, './dist/web/browser/index.html'));
});

app.use(function (req, res, next) {
	var err = new Error('Not Found');
	err.status = 404;
	next(err);
});

app.use(function (err, req, res, next) {
	res.locals.message = err.message;
	res.locals.error = req.app.get('env') === 'development' ? err : {};

	res.status(err.status || 500);
	console.log(err);
	res.send({ ok: false, error: err.message });
});

let port = 80;

app.listen(port, function () {
	console.log(`Web listening on port ${port}!`);
});
```

## GitLab CI

create file `.gitlab-ci.yml`

```yml
stages:
    - build
    - deploy
build:
    image: node:20-alpine
    stage: build
    script:
        - npm i
        - npm run build
deploy:
    image: docker:latest
    stage: deploy
    services:
        - docker:dind
    before_script:
        - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    script:
        - |
            if [[ "$CI_COMMIT_BRANCH" == "$CI_DEFAULT_BRANCH" ]]; then
              tag=""
              echo "Running on default branch '$CI_DEFAULT_BRANCH': tag = 'latest'"
            else
              tag=":$CI_COMMIT_REF_SLUG"
              echo "Running on branch '$CI_COMMIT_BRANCH': tag = $tag"
            fi
        - docker build --pull -t "$CI_REGISTRY_IMAGE${tag}" .
        - docker push "$CI_REGISTRY_IMAGE${tag}"
    only:
        - main
```

## API (fastify)

Create `process.json`

```json
{
	"apps": [
		{
			"name": "api",
			"script": "/app/dist/server.js",
			"instances": "max",
			"exec_mode": "cluster",
			"out_file": "/dev/null",
			"error_file": "/dev/null"
		}
	]
}
```

`Dockerfile`

```dockerfile
FROM node:20-alpine AS builder
LABEL maintainer="Satit Rianpit <rianpit@gmail.com>"
WORKDIR /app
RUN apk add --no-cache python3 g++
COPY . .
RUN npm i && npm run build
RUN npm i --omit=dev && npm rebuild bcrypt

FROM node:20-alpine
COPY --from=builder /app /app
WORKDIR /app
RUN apk add --no-cache g++
RUN npm i -g pm2
EXPOSE 3000

CMD ["pm2-runtime", "--json", "/app/process.json"]
```

Create `.gitlab-ci.yml`

```yml
stages:
    - build
    - deploy
build:
    image: node:20-alpine
    stage: build
    script:
        - npm i
        - npm run build
deploy:
    image: docker:latest
    stage: deploy
    services:
        - docker:dind
    before_script:
        - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    script:
        - |
            if [[ "$CI_COMMIT_BRANCH" == "$CI_DEFAULT_BRANCH" ]]; then
              tag=""
              echo "Running on default branch '$CI_DEFAULT_BRANCH': tag = 'latest'"
            else
              tag=":$CI_COMMIT_REF_SLUG"
              echo "Running on branch '$CI_COMMIT_BRANCH': tag = $tag"
            fi
        - docker build --pull -t "$CI_REGISTRY_IMAGE${tag}" .
        - docker push "$CI_REGISTRY_IMAGE${tag}"
    only:
        - main
```

## Running service

`nginx.conf`

```conf
error_log                       /var/log/nginx/error.log warn;

events {
    worker_connections          1024;
}

http {
  include                     /etc/nginx/mime.types;
  default_type                application/octet-stream;
  sendfile                    on;
  access_log                  /var/log/nginx/access.log;
  keepalive_timeout           300000;
  types_hash_max_size         300000;
  proxy_connect_timeout       300000;
  proxy_send_timeout          300000;
  proxy_read_timeout          300000;
  send_timeout                300000;

  server {
    listen 80;
    server_name r7platform-user.moph.go.th;
    client_max_body_size    50m;

    location / {
      proxy_pass http://web;
    }

    location /api/ {
      proxy_pass http://api:3000;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
      rewrite ^/api/(.*)$ /$1 break;
    }

  }

}
```

Create `docker-compose.yml`

```yml
version: '3'
services:
    nginx:
        image: nginx:stable-alpine
        ports:
            - 8888:80
        volumes:
            - ./nginx.conf:/etc/nginx/nginx.conf
        restart: always
        links:
            - web
            - api

    web:
        image: myrepro/web
        restart: always

    api:
        image: myrepro/api
        restart: always
        volumes:
            - config/.env:/app/dist/config/.env
```
