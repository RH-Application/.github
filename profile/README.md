# MMRE
## MicroServices Spring Boot with docker
A team of esprit students developing microservices for a project implementing an Odoo clone.

## ✨ Introductions

```sh
this is for educational purposes only 

```

## Authors

- [@MoetazBrayek](https://www.github.com/MoetazBrayek)
- [@MohamedBenHadid](https://www.github.com/MedBenHadid)
- [@eyaBenLil](https://www.github.com/eya02)
- [@RahmaChhoud](https://www.github.com/rahmachhoud)

  
## Tech Stack 💻

> [Node.js](https://nodejs.org/en/) <br />
> [Express.js](https://expressjs.com/) <br />
> [MongoDB ](https://www.mongodb.com/cloud/atlas) <br />
> [Material UI](https://material-ui.com/) <br />

**Client Side:** Angular 12 , RxJs, NgRx, NgPrime, RsocketClient

**Server Side:** Spring, Zuul, Eureka, MongoDB (reactive), Flux, RSocketServer ,Nodejs
![This is an image](https://cdn-images-1.medium.com/max/800/1*oxaA7PahX1-zo956FYLHFA.jpeg)

  
## Features

Our project is for Human Resource Management,it  contains 12 microservices with Zuul gateway and an Eureka server ,our microservices are in one container docker and they are using api from each other .

1. User Microservice
2. Employees microservice
3. Job microservice
4. Client microservice
5. Contact microservice

###### Description:

- User Microservice : manage user authentification and register
- Employees microservice : manage employees 
- Client microservice :manage clients 
- Job microservice :manage Job for admin
- Contact microservice:manage clients Contact


## Installation

Dillinger requires [Node.js](https://nodejs.org/) v12+ to run. <br />

Install the dependencies and devDependencies and start the Client. 

```sh
cd FitMe
cd client
yarn install
yarn start
```

Install the dependencies and devDependencies and start the Client.

```sh
cd FitMe
cd server
npm install
npm start
```

For production environments...

```sh
docker compose build
docker compose up
```
**1. Create a docker-compose.yml file in the same folder**
```
version: "2.2"
services:
 
  Eureka:
    container_name: serviceregistry
    build: .\Eureka-Server
    ports:
      - "8761:8761"
    hostname: serviceregistry
    image: "eureka"
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://serviceregistry:8761/eureka/
  
  Mongo:
    container_name: Mongo
    image: "mongo:5-focal"
    ports:
      - "27017:27017"
    hostname: Mongo
    depends_on:
      - "Eureka"
   
  Zuul:
    container_name: Zuul
    image: "zuul"
    build: .\spring-boot-zuul-gateway-proxy
    ports:
      - "8050:8050"
    hostname: Zuul
    environment:
      - EUREKA_CLIENT_SERVICEURL_DEFAULTZONE=http://serviceregistry:8761/eureka/
    depends_on:
      - "Eureka"
    
  employee:
    container_name: employee
    image: "employee"
    build: .\MS-Employee
    ports:
      - "5000:5000"
    hostname: employee
    depends_on:
      - "Eureka"
      - "Mongo"
    command: "node ./src/app.js"
    
  auth:
    container_name: auth
    image: "auth"
    build: .\MS-auth
    ports:
      - "7000:7000"
    hostname: auth
    depends_on:
      - "Eureka"
      - "Mongo"
    command: "node ./src/app.js"
    

  mysqldb:
    image: mysql:5.7
    restart: unless-stopped
    environment:
     - MYSQL_ROOT_PASSWORD=rootpass
     - MYSQL_DATABASE=micros
     - MYSQL_USER=eya
     - MYSQL_PASSWORD=eya
    ports:
     - "3306:3306"
    volumes:
     - .m2:/root/.m2
    stdin_open: true
    tty: true



  Client-ms:
    build: ./Client-ms
    image: client-ms
    ports:
     - 8081:8081
    depends_on:
     - Eureka
    environment:
     EUREKA_CLIENT_SERVICEURL_DEFAULTZONE: http://Eureka:8761/eureka
     SPRING_APPLICATION_JSON: '{
          "spring.datasource.url"  : "jdbc:mysql://mysqldb:3306/micros?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC",
          "spring.datasource.username" : "eya",
          "spring.datasource.password" : "eya",
          "spring.jpa.properties.hibernate.dialect" : "org.hibernate.dialect.MySQL5InnoDBDialect",
          "spring.jpa.hibernate.ddl-auto" : "update"
        }'

  job_A:
    build: ./Job-micro
    environment: 
      - eureka.client.serviceUrl.defaultZone=http://discovery:8761/eureka/
      - spring.cloud.config.uri=http://config:8888
    ports:
      - "3000:8082"
    image: "job-service"

```

**2. Create a .env file in Server  folder and add the following**

```
PORT = 5000
MONGO_URL= "mongodb://Mongo:27017/auth"
MAIL_SERVICE=
MAIL_USER= ""
MAIL_PASSWORD= ""
MAIL_PORT=587
MAIL_ADMIN= ""
ACCESS_TOKEN = 
HOSTNAME='localhost'
EUREKAPORT=8761
IPADD='127.0.0.1'
EUREKAHOST='serviceregistry'


```

 **🎉 Open your browser and go to `https://localhost:3000`**

## 🤩 Don't forget to give this repo a ⭐ if you like this repo and want to appreciate our efforts
