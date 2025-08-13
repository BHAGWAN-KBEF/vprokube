# VProfile Kubernetes Application

A multi-tier web application deployed on Kubernetes with Docker containerization. This project demonstrates a complete microservices architecture with database, caching, message queue, and web application components.

## 🏗️ Architecture

The application consists of the following components:

- **Web Server**: Nginx reverse proxy
- **Application Server**: Java web application running on Tomcat
- **Database**: MySQL database with persistent storage
- **Cache**: Memcached for application caching
- **Message Queue**: RabbitMQ for asynchronous messaging

## 📁 Project Structure

```
vprokube/
├── Docker-files/           # Docker configurations
│   ├── app/               # Application container
│   │   ├── Dockerfile     # Tomcat-based Java app
│   │   └── multistage/    # Multi-stage build option
│   ├── db/                # Database container
│   │   ├── Dockerfile     # MySQL configuration
│   │   └── db_backup.sql  # Database initialization
│   └── web/               # Web server container
│       ├── Dockerfile     # Nginx configuration
│       └── nginvproapp.conf # Nginx proxy config
├── kubedefs/              # Kubernetes manifests
│   ├── appdeploy.yaml     # Application deployment
│   ├── appservice.yaml    # Application service
│   ├── appingress.yaml    # Ingress configuration
│   ├── dbdeploy.yaml      # Database deployment
│   ├── dbservice.yaml     # Database service
│   ├── dbpvc.yaml         # Database persistent volume
│   ├── mcdep.yaml         # Memcached deployment
│   ├── mcservice.yaml     # Memcached service
│   ├── rmqdeploy.yaml     # RabbitMQ deployment
│   ├── rmqservice.yaml    # RabbitMQ service
│   └── secret.yaml        # Kubernetes secrets
└── docker-compose.yml     # Local development setup
```

## 🚀 Quick Start

### Prerequisites

- Docker and Docker Compose
- Kubernetes cluster (local or cloud)
- kubectl configured
- Nginx Ingress Controller

### Local Development with Docker Compose

```bash
# Clone the repository
git clone <repository-url>
cd vprokube

# Start all services
docker-compose up -d

# Access the application
# Web: http://localhost
# App: http://localhost:8080
```

### Kubernetes Deployment

1. **Deploy the database layer:**
```bash
kubectl apply -f kubedefs/secret.yaml
kubectl apply -f kubedefs/dbpvc.yaml
kubectl apply -f kubedefs/dbdeploy.yaml
kubectl apply -f kubedefs/dbservice.yaml
```

2. **Deploy supporting services:**
```bash
kubectl apply -f kubedefs/mcdep.yaml
kubectl apply -f kubedefs/mcservice.yaml
kubectl apply -f kubedefs/rmqdeploy.yaml
kubectl apply -f kubedefs/rmqservice.yaml
```

3. **Deploy the application:**
```bash
kubectl apply -f kubedefs/appdeploy.yaml
kubectl apply -f kubedefs/appservice.yaml
kubectl apply -f kubedefs/appingress.yaml
```

## 🔧 Configuration

### Environment Variables

- `MYSQL_ROOT_PASSWORD`: Database root password (stored in Kubernetes secret)
- `RABBITMQ_DEFAULT_USER`: RabbitMQ username
- `RABBITMQ_DEFAULT_PASS`: RabbitMQ password

### Resource Limits

The application is configured with resource requests and limits:

- **Application Pod**: 512Mi-1Gi memory, 300m-600m CPU
- **Database Pod**: 256Mi-512Mi memory, 250m-500m CPU
- **Init Containers**: 64Mi-128Mi memory, 100m-200m CPU

### Ingress Configuration

The application is accessible via ingress at `project.kbefkbef.online` with Nginx ingress controller.

## 🔍 Monitoring & Health Checks

The application includes:

- Init containers for dependency checking
- Resource limits and requests
- Persistent volume for database data
- Service discovery between components

## 🛠️ Development

### Building Custom Images

```bash
# Build application image
docker build -t vprocontainers/vprofileapp ./Docker-files/app/

# Build database image
docker build -t vprocontainers/vprofiledb ./Docker-files/db/

# Build web server image
docker build -t vprocontainers/vprofileweb ./Docker-files/web/
```

### Multi-stage Build

For optimized production builds, use the multi-stage Dockerfile:

```bash
docker build -t vprocontainers/vprofileapp ./Docker-files/app/multistage/
```

## 📊 Services & Ports

| Service | Port | Description |
|---------|------|-------------|
| Web (Nginx) | 80 | Reverse proxy |
| Application (Tomcat) | 8080 | Java web application |
| Database (MySQL) | 3306 | Database server |
| Cache (Memcached) | 11211 | Caching service |
| Message Queue (RabbitMQ) | 5672 | Message broker |

## 🔐 Security

- Database credentials stored in Kubernetes secrets
- Resource limits prevent resource exhaustion
- Init containers ensure proper startup sequence
- Network policies can be added for additional security

## 📝 Notes

- The application uses init containers to ensure dependencies are ready before starting
- Persistent volumes ensure database data survives pod restarts
- The setup supports both local development (Docker Compose) and production (Kubernetes)
- Nginx acts as a reverse proxy to the Tomcat application server

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally with Docker Compose
5. Test on Kubernetes
6. Submit a pull request

## 📄 License

This project is part of a DevOps learning exercise demonstrating containerization and Kubernetes deployment patterns.