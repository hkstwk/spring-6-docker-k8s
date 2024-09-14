## CREATE image for project
```
./mvnw clean package spring-boot:build-image
```

## IMAGES IN docker.io/library/
- spring-6-gateway:0.0.1-SNAPSHOT
- spring-6-auth-server:0.0.1-SNAPSHOT
- spring-6-rest-mvc:0.0.1-SNAPSHOT
- spring-6-reactive:0.0.1-SNAPSHOT
- reactive-mongo:0.0.1-SNAPSHOT

## Run Gateway
```
docker run --name gateway -d -p 8080:8080 spring-6-gateway:0.0.1-SNAPSHOT
```

## Run gateway, active profile docker
```
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker spring-6-gateway:0.0.1-SNAPSHOT
```

## Run gateway, active profile docker, link auth-server
```
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth-server:auth-server spring-6-gateway:0.0.1-SNAPSHOT
```

## Run gateway, active profile docker, link auth-server, link rest-mvc
```
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth-server:auth-server --link rest-mvc:rest-mvc spring-6-gateway:0.0.1-SNAPSHOT
```

## Run gateway, active profile docker, link auth-server, link rest-mvc, link reactive
```
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker \
 --link auth-server:auth-server --link rest-mvc:rest-mvc --link reactive:reactive \
 spring-6-gateway:0.0.1-SNAPSHOT
```

## Run Auth-server
```
docker run --name auth-server -d -p 9000:9000 spring-6-auth-server:0.0.1-SNAPSHOT
```

## Run rest-mvc
```
docker run --name rest-mvc -d -p 8080:8080 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 --link auth-server:auth-server spring-6-rest-mvc:0.0.1-SNAPSHOT
```

## Run MySql 
```
docker run --name spring-6-mysql -d -e MYSQL_USER=restadmin -e MYSQL_PASSWORD=password -e MYSQL_DATABASE=restdb -e MYSQL_ROOT_PASSWORD=password mysql:latest
```

## Run rest-mvc, link mysql
```
docker stop rest-mvc
```
```
docker rm rest-mvc
```
```
docker run --name rest-mvc -d -p 8082:8080 -e SPRING_PROFILES_ACTIVE=localmysql \
-e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 -e SPRING_DATASOURCE_URL=jdbc:mysql://spring-6-mysql:3306/restdb \
-e SERVER_PORT=8080 --link auth-server:auth-server --link spring-6-mysql:spring-6-mysql spring-6-rest-mvc:0.0.1-SNAPSHOT
```

## Run Reactive
```
docker run --name reactive -d -p 8083:8080 -e SERVER_PORT=8080 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 \
--link auth-server:auth-server spring-6-reactive:0.0.1-SNAPSHOT
```

## Run MongoDB
```
docker run --name mongo -d -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=example -p 27017:27017 mongo
```

## Run Reactive Mongo
```
docker run --name reactive-mongo -d -p 8084:8080 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 \
-e SERVER_PORT=8080 -e SFG_MONGOHOST=mongo --link auth-server:auth-server --link mongo:mongo reactive-mongo:0.0.1-SNAPSHOT
```

## Run gateway active profile docker link auth-server link rest-mvc bash command
```
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth-server:auth-server \
--link rest-mvc:rest-mvc --link reactive-mongo:reactive-mongo spring-6-gateway:0.0.1-SNAPSHOT
```
