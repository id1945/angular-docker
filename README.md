# [1] Dockerfile local
### [1.1] Dockerfile
```yml
FROM node:14-stretch-slim as build
WORKDIR /app
COPY ./ /app
RUN npm install && npm run build

FROM nginx:latest
COPY --from=build /app/dist/angular-docker /usr/share/nginx/html
```
### [1.2] Virtual machine
#### Create image
```bash
docker build -t angular_build -f Dockerfile .
```
#### Run container
```bash
docker run -d --name angular_host -p 8080:80 angular_build
```
```bash
docker ps -la
docker stop angular_host
docker start angular_host
docker rm angular_host
```
```utl
Test: http://localhost:8080/
```
# [2] Dockerfile on docker-hub
### [2.1] Push Dockerfile to docker-hub
```bash
docker tag angular-demo:latest id1945/angular-demo
docker push id1945/angular-demo
```
```url
Check: https://hub.docker.com/r/id1945/angular-demo
```
### [2.2] Run container by docker-compose
#### docker-compose.yml
```yml
version: "3.3"
services:
  web-angular:
    image: id1945/angular-demo:latest
    ports:
      - "8080:80"
```
```bash
docker-compose up -d
```
# [3] Run docker-compose with ssh file
#### deploy.ssh
```yml
docker-compose stop
docker-compose rm -f
docker-compose pull
docker-compose up -d
```
#### Run deploy.ssh
```bash
chmod +x deploy.sh
./deploy.sh
```
#### What?
-d (Run in background)
Refer: https://giai-ma.blogspot.com/2020/08/tim-hieu-docker-images-containers.html 

#### Author: DaiDH Tel: 0845882882
