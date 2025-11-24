# Spring Boot CI/CD with Azure DevOps

![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=for-the-badge&logo=azure-devops&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

[![Build status](https://houssemdellai.visualstudio.com/Java-SpringBoot-WebApp/_apis/build/status/Java-SpringBoot-Maven-CI)](https://houssemdellai.visualstudio.com/Java-SpringBoot-WebApp/_build/latest?definitionId=96)

A production-ready **Java Spring Boot** web application demonstrating complete **CI/CD automation** using **Azure DevOps Pipelines** with automated build, test, and deployment to **Azure App Service**.

## 🎯 Overview

This project showcases:
- ✅ Spring Boot web application development with Java
- ✅ Multi-stage Azure DevOps CI/CD pipeline
- ✅ Maven-based build automation
- ✅ Automated deployment to Azure App Service (Linux)
- ✅ Docker containerization support
- ✅ Infrastructure as Code with YAML pipelines
- ✅ Artifact management and versioning
- ✅ Environment-based deployment strategies

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                  Azure DevOps Pipeline                       │
│                                                              │
│  ┌────────────┐      ┌────────────┐      ┌────────────┐    │
│  │   BUILD    │─────▶│   TEST     │─────▶│  ARTIFACT  │    │
│  │  • Maven   │      │  • JUnit   │      │  • JAR     │    │
│  │  • Compile │      │  • Reports │      │  • Publish │    │
│  └────────────┘      └────────────┘      └────────────┘    │
│                                                 │            │
└─────────────────────────────────────────────────┼────────────┘
                                                  │
                                                  ▼
                               ┌────────────────────────────────┐
                               │     DEPLOY STAGE               │
                               │  ┌──────────────────────────┐  │
                               │  │   Download Artifacts     │  │
                               │  └──────────────────────────┘  │
                               │            ▼                    │
                               │  ┌──────────────────────────┐  │
                               │  │  Azure App Service       │  │
                               │  │  (Linux Container)       │  │
                               │  │  • Spring Boot App       │  │
                               │  │  • Port 8080             │  │
                               │  └──────────────────────────┘  │
                               └────────────────────────────────┘
```

## 🚀 Features

### Application Features
- **Spring Boot Framework**: Modern Java web application framework
- **RESTful APIs**: HTTP endpoints for web services
- **Maven Build**: Standardized dependency management
- **Gradle Support**: Alternative build system available
- **Embedded Tomcat**: Self-contained web server

### DevOps & CI/CD
- **Automated Build**: Maven/Gradle compilation on every commit
- **Continuous Integration**: Automatic testing and validation
- **Continuous Deployment**: Auto-deploy to Azure App Service
- **Multi-Stage Pipeline**: Separate build and deploy stages
- **Artifact Management**: Versioned build artifacts
- **Environment Variables**: Configurable deployment settings

### Containerization
- **Docker Support**: Multi-stage Dockerfile for optimization
- **Maven Build Container**: Isolated build environment
- **Container Registry**: Push to Azure Container Registry
- **Kubernetes Ready**: Deploy to AKS with minor modifications

## 📋 Prerequisites

### Development
- **Java JDK** >= 11
- **Maven** >= 3.6 or **Gradle** >= 6.8
- **Git** for version control
- **IDE**: IntelliJ IDEA, Eclipse, or VS Code

### Deployment
- **Azure Account** with active subscription
- **Azure DevOps** organization and project
- **Azure CLI** installed and authenticated
- **Azure App Service** (Linux) instance

## 🔧 Local Development

### Build with Maven

```bash
# Clone the repository
git clone https://github.com/rajdip-chakraborty-dev/gs-spring-boot.git
cd gs-spring-boot

# Build the project
cd app
./mvnw clean package

# Run the application
java -jar target/*.jar

# Or run directly with Maven
./mvnw spring-boot:run
```

### Build with Gradle

```bash
cd app
./gradlew clean build

# Run the application
./gradlew bootRun
```

### Test the Application

```bash
# Application runs on port 8080 by default
curl http://localhost:8080

# Run tests
./mvnw test
```

## 🐳 Docker Build & Run

### Build Docker Image

```bash
# Build using Maven container
docker build -t spring-boot-app:latest .

# Or build multi-stage for production
docker build -t spring-boot-app:prod -f Dockerfile.multistage .
```

### Run Container

```bash
# Run the container
docker run -d -p 8080:8080 --name spring-app spring-boot-app:latest

# Test the application
curl http://localhost:8080

# View logs
docker logs -f spring-app
```

## 🔄 CI/CD Pipeline

### Azure DevOps Pipeline Configuration

The project includes **three pipeline configurations**:

1. **`azure-pipelines.yml`** - Complete CI/CD with Build + Deploy
2. **`AzurePipelines-CI.yaml`** - Continuous Integration only
3. **`AzurePipelines-CI-CD.yaml`** - Full CI/CD with multiple environments

### Pipeline Stages

#### 1️⃣ Build Stage
```yaml
- Maven compilation
- Run unit tests
- Package JAR/WAR artifact
- Copy files to staging
- Publish pipeline artifacts
```

#### 2️⃣ Deploy Stage
```yaml
- Download build artifacts
- Deploy to Azure App Service
- Configure app settings
- Validate deployment
```

### Pipeline Variables

Configure these in Azure DevOps:

| Variable | Description | Example |
|----------|-------------|---------|
| `azureSubscription` | Azure service connection | `guid-string` |
| `webAppName` | Azure App Service name | `java-spring-boot` |
| `vmImageName` | Build agent OS | `ubuntu-latest` |

### Setting Up the Pipeline

1. **Create Azure App Service**
   ```bash
   az webapp create \
     --resource-group myResourceGroup \
     --plan myAppServicePlan \
     --name java-spring-boot \
     --runtime "JAVA:11-java11"
   ```

2. **Create Service Connection**
   - In Azure DevOps: Project Settings → Service connections
   - Add new Azure Resource Manager connection
   - Copy the connection ID to `azureSubscription` variable

3. **Import Pipeline**
   - Pipelines → New Pipeline → Azure Repos Git
   - Select repository
   - Choose existing Azure Pipelines YAML file
   - Select `azure-pipelines.yml`

4. **Run Pipeline**
   - Save and run
   - Monitor build and deployment stages
   - Access deployed app at: `https://java-spring-boot.azurewebsites.net`

## 📁 Project Structure

```
gs-spring-boot/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   └── java/
│   │   │       └── hello/          # Spring Boot application code
│   │   └── test/
│   │       └── java/
│   │           └── hello/          # Unit tests
│   ├── pom.xml                      # Maven dependencies
│   ├── build.gradle                 # Gradle build config
│   ├── mvnw / mvnw.cmd             # Maven wrapper
│   └── gradlew / gradlew.bat       # Gradle wrapper
├── test/
│   └── run.sh                       # Test execution script
├── Dockerfile                       # Docker build instructions
├── azure-pipelines.yml              # Main CI/CD pipeline
├── AzurePipelines-CI.yaml          # CI-only pipeline
├── AzurePipelines-CI-CD.yaml       # Advanced CI/CD pipeline
├── build.sh                         # Build automation script
└── README.md                        # This file
```

## 🚢 Deployment Options

### 1. Azure App Service (Linux)

**Via Azure CLI:**
```bash
az webapp deployment source config-zip \
  --resource-group myResourceGroup \
  --name java-spring-boot \
  --src target/app-0.1.0.jar
```

**Via Azure DevOps:**
- Automatic deployment on merge to `master` branch
- Configured in `azure-pipelines.yml`

### 2. Azure Container Instances

```bash
# Build and push to ACR
az acr build --registry myregistry --image spring-boot-app:v1 .

# Deploy to ACI
az container create \
  --resource-group myResourceGroup \
  --name spring-boot-app \
  --image myregistry.azurecr.io/spring-boot-app:v1 \
  --dns-name-label spring-boot-demo \
  --ports 8080
```

### 3. Azure Kubernetes Service (AKS)

```yaml
# kubernetes-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-boot-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: spring-boot
  template:
    metadata:
      labels:
        app: spring-boot
    spec:
      containers:
      - name: spring-boot
        image: myregistry.azurecr.io/spring-boot-app:v1
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: spring-boot-service
spec:
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 8080
  selector:
    app: spring-boot
```

Deploy:
```bash
kubectl apply -f kubernetes-deployment.yaml
```

### 4. Docker Hub

```bash
# Build and tag
docker build -t username/spring-boot-app:latest .

# Push to Docker Hub
docker push username/spring-boot-app:latest

# Run from Docker Hub
docker run -d -p 8080:8080 username/spring-boot-app:latest
```

## ⚙️ Configuration

### Application Properties

Edit `app/src/main/resources/application.properties`:

```properties
# Server port
server.port=8080

# Application name
spring.application.name=spring-boot-demo

# Logging
logging.level.root=INFO
logging.level.com.example=DEBUG
```

### Environment Variables

Set in Azure App Service:
```bash
az webapp config appsettings set \
  --resource-group myResourceGroup \
  --name java-spring-boot \
  --settings JAVA_OPTS="-Xms512m -Xmx1024m"
```

## 🧪 Testing

### Run Unit Tests

```bash
cd app

# Maven
./mvnw test

# Gradle
./gradlew test
```

### Generate Test Reports

```bash
# Maven with coverage
./mvnw test jacoco:report

# View report at: target/site/jacoco/index.html
```

### Integration Tests

```bash
./mvnw verify
```

## 📊 Monitoring & Logging

### Azure Application Insights

Add to `pom.xml`:
```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>2.6.4</version>
</dependency>
```

Configure:
```properties
azure.application-insights.instrumentation-key=${APPINSIGHTS_KEY}
```

### View Logs

```bash
# Azure CLI
az webapp log tail \
  --resource-group myResourceGroup \
  --name java-spring-boot

# Docker logs
docker logs -f spring-app
```

## 🔍 Troubleshooting

### Build Fails

```bash
# Clean Maven cache
./mvnw clean

# Force update dependencies
./mvnw clean install -U
```

### Deployment Issues

```bash
# Check Azure App Service logs
az webapp log show \
  --resource-group myResourceGroup \
  --name java-spring-boot

# Check container status
az webapp show \
  --resource-group myResourceGroup \
  --name java-spring-boot \
  --query state
```

### Port Conflicts

```bash
# Change port in application.properties
server.port=8081

# Or set environment variable
export SERVER_PORT=8081
```

## 📚 Resources

- **Azure DevOps Pipelines**: https://houssemdellai.visualstudio.com/Java-SpringBoot-WebApp
- **Spring Boot Documentation**: https://spring.io/projects/spring-boot
- **Azure App Service Docs**: https://docs.microsoft.com/azure/app-service/
- **Maven Guide**: https://maven.apache.org/guides/
- **Docker Hub**: https://hub.docker.com/_/openjdk

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

## 📄 License

This project is open source and available under standard licensing terms.

## 👤 Author

**Rajdip Chakraborty**
- GitHub: [@rajdip-chakraborty-dev](https://github.com/rajdip-chakraborty-dev)
- LinkedIn: [Rajdip Chakraborty](https://www.linkedin.com/in/rajdip-chakraborty)

## ⭐ Show Your Support

Give a ⭐️ if this project helped you learn about Spring Boot CI/CD with Azure DevOps!

---

**Tags**: `spring-boot` `java` `azure-devops` `ci-cd` `maven` `gradle` `docker` `azure-app-service` `kubernetes`

