# Sample Spring Boot Application
This is a sample Spring Boot application that demonstrates how to build a simple REST API with Spring Boot.

## Requirements
- Java 8
- Maven
- Docker
- Docker Compose
- Git
- IDE (IntelliJ IDEA, Eclipse, etc.)

## Installation
1. Clone the repository
```bash
git clone
```

2. Change directory
```bash
cd spring-boot-sample
```

3. Build the application
```bash
mvn clean install
```

4. Run the application
```bash
mvn spring-boot:run
```

5. Access the application
```
http://localhost:8080
```

## Docker
1. Build the Docker image
```bash
docker build -t spring-boot-sample .
```

2. Run the Docker container
```bash

docker run -p 8080:8080 spring-boot-sample
```

3. Access the application
```
http://localhost:8080
```

## Docker Compose
1. Run the Docker Compose
```bash
docker-compose up -d
```

2. Access the application
```
http://localhost:8080
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```