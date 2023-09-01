# Spring Cloud API Gateway adding to Eureka Server
1 Create normal springBoot application using start.spring.io
    https://start.spring.io/

2 Add Eureka discovery client dependency to pom file 

    <dependency>
        <groupId>io.pivotal.spring.cloud</groupId>
        <artifactId>spring-cloud-services-starter-service-registry</artifactId>
    </dependency>
        
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-gateway</artifactId>
    </dependency>

    <dependency>
        <groupId>io.projectreactor</groupId>
        <artifactId>reactor-test</artifactId>
        <scope>test</scope>
    </dependency>
    
3 Annotate the main class with `@EnableEurekaDiscoveryClient` 
    
4 Configure application.properties file as following

    #Giving port value 0 will make the eureka server to assign the port automatically
    server.port=8082
    spring.application.name=apigateway-ws
    
    #The following property will help communicate with eureka server
    eureka.client.serviceUrl.defaultZone = http://localhost:8010/eureka
    
    #The following properties works to routing purpose
    #Path to the resource
    spring.cloud.gateway.routes[0].id=users-status
    
    #This property used by spring cloud to identify the microservice to get route to
    #Array list of microservices
    spring.cloud.gateway.routes[0].uri=lb://users-ws
    
    #This will match the path to specific microservice resource and it uses java 8 predicate value for identification
    spring.cloud.gateway.routes[0].predicates[0]=Path=/users/status
    
    #This will restrict which method of http are allowed to the resource.
    spring.cloud.gateway.routes[0].predicates[1]=Method=GET
    
    #remove the microservice having cookie
    spring.cloud.gateway.routes[0].filters[0]=RemoveRequestHeader=Cookie
    
    spring.cloud.gateway.discovery.locator.enabled=true
    #
    ##This will make the gateway to match lower case and upper case
    spring.cloud.gateway.discovery.locator.lower-case-service-id=true
    
5 First start Eureka server application then start the other microservice application and API gateway application

6 Open postman and send request the microservice through the API gateway
    `http://localhost:8082/users-ws/users/status` 

* If every thing go fine the microservice will return response through the API gateway. 
