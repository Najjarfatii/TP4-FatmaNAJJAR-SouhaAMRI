# TP4-FatmaNAJJAR-SouhaAMRI
                  Microservices Dockerization Tutorial
How to Run it
  *clone the project
  *run docker-compose up
Requirements

    docker
    docker composer
    
Structure of the repository

The project includes four modules,maven projects, coded in spring boot.
In addition, the Dockerfile defines multi-stage images build and the docker-compose.yml which defines the containers to be instantiated. Finally we used the file "wait-for-it.sh" which is cloned from the following project "https://github.com/vishnubob/wait-for-it".

Dockerization

Docker-compose.yml

The dockerfile will run only one time and then each stage mentioned in the docker-compose.yml will be used to create the corresponding container.

    config-service

  config-service:
    build: config-service
    ports: 
     - "8888:8888"
  discovery-service:
    build: discovery-service
    ports: 
     - "8761:8761"
    depends_on:
      - config-service
	 entrypoint: /wait-for-it.sh config-service:8888 -t 300 --
    command: java -jar /usr/app/discovery-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
    
   proxy-service
   
   proxy-service:
    build: proxy-service
    ports: 
     - "9999:9999"
    depends_on:
      - discovery-service
	    entrypoint: /wait-for-it.sh discovery-service:8761 -t 300 --
    command: java -jar /usr/app/proxy-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
    
    discovery-service
    
  discovery-service:
    build: discovery-service
    ports: 
     - "8761:8761"
    depends_on:
      - config-service
	 entrypoint: /wait-for-it.sh config-service:8888 -t 300 --
    command: java -jar /usr/app/discovery-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
    
    
    product-services
    
   product-service-8080:
    build: product-service
    ports: 
     - "8080:8080"
    depends_on:
      - proxy-service
    entrypoint: /wait-for-it.sh proxy-service:9999 -t 300 --
    command: java -jar /usr/app/product-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
  product-service-8081:
    build: product-service
    ports: 
     - "8081:8080"
    depends_on:
      - proxy-service
    entrypoint: /wait-for-it.sh proxy-service:9999 -t 300 --
    command: java -jar /usr/app/product-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
  product-service-8082:
    build: product-service
    ports: 
     - "8082:8080"
    depends_on:
      - proxy-service
    entrypoint: /wait-for-it.sh proxy-service:9999 -t 300 --
    command: java -jar /usr/app/product-service-0.0.1-SNAPSHOT.jar --spring.profiles.active=docker
    
    Contributors
     Fatma NAJJAR
     Souha AMRI
    
    
    
 
 
 
