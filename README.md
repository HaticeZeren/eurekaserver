Bu proje, Eureka Server uygulama örneğidir.

https://medium.com/@haticezeren0/eureka-server-ve-zuul-edge-server-nedir-spring-boot-uygulama-%C3%B6rne%C4%9Fi-fdf60df0202e

# Gereksinimler:
- Docker
- Docker Compose
- IntelliJ

# Projenin Çalıştırılması

## Docker Image'larının Oluşturulması

  Projeyi lokalimize çekelim maven ile clean-install yapalım ve intelliJ'e ait terminalde aşağıdaki komutu yazarak stock-service için image oluşturalım.

`    docker build -t eureka-server:0.0.1 .
`

##  Docker Compose File dosyası ile deploy etme

-  eureka-server.yml dosyası aşağıdaki gibidir.

```
version: '3.3'

services:
  eureka-server:
    container_name: eureka-server
    hostname: eureka-server
    image: eureka-server:0.0.1
    networks:
      - springboot-mysql-network
    expose:
      - 8761
    ports:
      - 8761:8761
    deploy:
      replicas: 1
      restart_policy:
        condition: on-failure
        delay: 5s
        max_attempts: 3
        window: 120s
      resources:
        reservations:
          cpus: "0.50"
          memory: 512M
        limits:
          cpus: "1.0"
          memory: 1G
    environment:
      - "SPRING_PROFILES_ACTIVE=stage"
    entrypoint: [ "java","-jar","app.jar" ]

networks:
  springboot-mysql-network:
    name: springboot-mysql-network

```

Docker deploy komutu aşağıdaki gibidir:

> docker-compose -p eureka-example  -f eureka-server.yml up -d



Docker deploy komutu aşağıdaki gibidir:

> docker-compose -p eureka-example  -f eureka-server.yml  up -d

