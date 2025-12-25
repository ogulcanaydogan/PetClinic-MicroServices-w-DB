# Spring PetClinic Microservices Platform (Spring Cloud)

Production-style microservices implementation of the Spring PetClinic application. This project demonstrates how to design, run, and operate a distributed system using the Spring Cloud ecosystem, focusing on service discovery, centralized configuration, routing, resilience, and observability rather than application business logic.

## Overview
The application is decomposed into multiple Spring Boot microservices supported by platform services such as configuration management, service discovery, API gateway, monitoring, and tracing. The system can be executed locally for development or fully containerized using Docker Compose to simulate a production-like environment.

This repository is intended as a reference for DevOps and platform engineers working with Java-based microservices.

## Core Components

### Application Services
- Customers Service
- Vets Service
- Visits Service
- API Gateway (AngularJS frontend entry point)

### Platform Services
- Config Server (centralized configuration)
- Discovery Server (Eureka)
- API Gateway (Zuul)
- Circuit Breaker (Hystrix)
- Tracing (Zipkin)
- Monitoring (Prometheus, Grafana)
- Admin Server (Spring Boot Admin)

## Architecture

![Spring PetClinic Microservices Architecture](docs/microservices-architecture-diagram.jpg)

Traffic flow:
Client -> API Gateway -> Backend Services -> Database  
API Gateway -> Service Discovery and Config Server

## Running Services Locally (Without Docker)
Each microservice is a standalone Spring Boot application and can be started using an IDE or Maven.

Command:
../mvnw spring-boot:run

Startup order:
1. Config Server
2. Discovery Server
3. Backend services (Customers, Vets, Visits)
4. API Gateway

Optional services:
- Zipkin
- Spring Boot Admin
- Prometheus
- Grafana

Default endpoints:
- Eureka Dashboard: http://localhost:8761
- Config Server: http://localhost:8888
- API Gateway / UI: http://localhost:8080
- Zipkin: http://localhost:9411/zipkin
- Admin Server: http://localhost:9090
- Grafana: http://localhost:3000
- Prometheus: http://localhost:9091
- Hystrix Dashboard: http://localhost:7979

## Running Services with Docker Compose
The entire platform can be started using Docker Compose.

Build Docker images:
./mvnw clean install -PbuildDocker

Start services:
docker-compose up

Service startup order is coordinated automatically. Initial API Gateway timeouts may occur until all services are registered in Eureka.

Note: On macOS or Windows, Docker Desktop memory allocation should be increased to ensure stable startup.

## Configuration Management
Configuration is centralized using Spring Cloud Config.

To use a local configuration repository, start services with:
-Dspring.profiles.active=local -DGIT_REPO=/path/to/config-repo

## Database Configuration

### Default Database
- In-memory HSQLDB
- Automatically populated with sample data at startup

### MySQL (Persistent Database)
MySQL can be started using Docker:
docker run -e MYSQL_ROOT_PASSWORD=petclinic -e MYSQL_DATABASE=petclinic -p 3306:3306 mysql:5.7.8

Enable MySQL profile when starting services:
--spring.profiles.active=mysql

For Docker-based deployments, the MySQL profile is enabled via environment variables in Dockerfiles:
SPRING_PROFILES_ACTIVE=docker,mysql

## Observability and Monitoring

### Metrics
- Micrometer instrumentation
- JVM, HTTP request, and custom business metrics

### Monitoring Stack
- Prometheus for metrics collection
- Grafana for dashboard visualization

![Grafana Metrics Dashboard](docs/grafana-custom-metrics-dashboard.png)

### Distributed Tracing
- Zipkin is used to trace requests across microservices

## Load Testing
A JMeter test plan is included to generate load and metrics:
spring-petclinic-api-gateway/src/test/jmeter/petclinic_test_plan.jmx

## What This Project Demonstrates
- Microservices architecture using Spring Cloud
- Service discovery and dynamic routing
- Centralized configuration management
- Circuit breaker and fault tolerance patterns
- Distributed tracing and metrics
- Operating a complete microservices platform locally or in containers

## Production Notes and Improvements
- Replace Zuul with Spring Cloud Gateway
- Migrate deprecated Netflix OSS components
- Deploy on Kubernetes (for example, Amazon EKS) instead of Docker Compose
- Externalize secrets using Vault or AWS Secrets Manager
- Add CI/CD pipelines for automated build and deployment

## References
- Spring PetClinic Microservices: https://github.com/spring-petclinic/spring-petclinic-microservices
- Spring Cloud Documentation: https://spring.io/projects/spring-cloud
- Micrometer: https://micrometer.io
- Spring Boot Actuator: https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-metrics.html

## Contributing
Issues and pull requests should be submitted via the upstream Spring PetClinic Microservices repository.
