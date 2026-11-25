# my1
[20:59, 25/11/2026] Sphoorthy: 1. aws

FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM tomcat:9-jdk17

COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]

----------------------------------------------------------------

2.docker-compose.yml

services:

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root123
      MYSQL_DATABASE: ecommerce
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wp123
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wp123
      WORDPRESS_DB_NAME: ecommerce
    volumes:
      - wp_data:/var/www/html
    depends_on:
      - db

volumes:
  db_data:
  wp_data:
-----------------------
services:
  web:
    build: .
    container_name: maven-container
    ports:
      - "3001:8080"
--------------------------------------------------------------------

3.docker-cli


FROM maven:3.9.9-eclipse-temurin-8 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM tomcat:9.0-jdk8-temurin
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=build /app/target/maven_web_java.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]

4.Scripted pipeline

node {

    stage('Git Checkout') {
        git branch: 'main',
            url: 'https://github.com/sphoorthypothuganti2429-beep/javaproject.git'
    }

    stage('Clean') {
        bat 'mvn clean'
    }

    stage('Compile') {
        bat 'mvn compile'
    }

    stage('Test') {
        bat 'mvn test'
    }

    stage('Package') {
        bat 'mvn package'
    }

    stage('Archive') {
        archiveArtifacts artifacts: 'target/*.jar',
                         fingerprint: true
    }

    echo 'BUILD SUCCESSFUL!'
}
----------------------------------------------------------------------------------------------------------------------------------------------------
5.docker cli commands
docker --version
docker pull redis
docker run --name my-redis -d redis
docker ps
docker exec -it my-redis redis-cli
docker stop my-redis
docker rmi my-redis
----------------------    ###dockerfile also
Dockerfile
FROM redis:latest
CMD["redis-server"]
----------------------
cd /c/DockerProjects/Redis
docker build -t redisnew .
docker run --name myredisnew -d redisnew
docker ps 
docker stop myredisnew
docker login
docker ps -a
docker commit 09e-------- myredisnew/redis1
docker images
docker build -t rukmni2006/redis1:latest
docker pull rukmini2004/redis1:latest
docker stop myredisnew
docker rm myredisnew
docker ps -a
docker logout
docker pull rukkmini2006/redis1
docker exec -it redis-cli
docker rmi rukmini2006/redis1
docker logout

