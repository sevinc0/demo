# DevOps Demo Project

Bu proje, bir Spring Boot uygulamasının modern DevOps araçları kullanılarak geliştirilmesi, container haline getirilmesi ve Kubernetes üzerinde çalıştırılması amacıyla hazırlanmıştır.

## 🚀 Kullanılan Teknolojiler

* Java 17
* Spring Boot
* Maven
* PostgreSQL
* Docker
* Docker Compose
* Jenkins
* Kubernetes
* Helm
* ArgoCD
* Git / GitHub

## 📌 Proje Yapısı

```text
demo/
├── src/
├── deployment.yaml
├── service.yaml
├── docker-compose.yml
├── Dockerfile
├── Jenkinsfile
├── pom.xml
└── README.md
```

## 🔹 Uygulama

Uygulama Spring Boot ile geliştirilmiş basit bir REST API uygulamasıdır.

PostgreSQL veritabanı Docker container olarak çalıştırılmış ve Spring Boot uygulaması PostgreSQL'e bağlanmıştır.

Örnek endpoint:

```text
GET /api/tasks
```

Sağlık kontrolü:

```text
GET /actuator/health
```

## 🔹 Docker

Uygulama Dockerfile kullanılarak image haline getirilmiştir.

Örnek:

```bash
docker build -t demo-app:1.0 .
```

Container çalıştırma:

```bash
docker run -d \
  --name demo-app-container \
  --network demo-network \
  -p 8082:8080 \
  demo-app:1.0
```

Container kontrolü:

```bash
docker ps
```

Log kontrolü:

```bash
docker logs demo-app-container
```

## 🔹 PostgreSQL

PostgreSQL Docker container üzerinde çalıştırılmıştır.

Spring Boot uygulamasının bağlantı bilgileri:

```properties
spring.datasource.url=jdbc:postgresql://demo-postgres:5432/demo_db
spring.datasource.username=demo_user
spring.datasource.password=demo_password
```

Docker network sayesinde uygulama PostgreSQL container'ına `demo-postgres` adı üzerinden bağlanmaktadır.

## 🔹 Jenkins

Jenkins kullanılarak CI/CD pipeline oluşturulmuştur.

Pipeline içerisinde temel olarak:

1. GitHub repository'sinden kod çekilir.
2. Maven ile proje build edilir.
3. Docker image oluşturulur.
4. Docker container deploy edilir.
5. Health check yapılır.

Pipeline akışı:

```text
GitHub
   ↓
Jenkins
   ↓
Maven Build
   ↓
Docker Build
   ↓
Docker Container
   ↓
Health Check
```

Jenkinsfile repository içerisinde bulunmaktadır.

## 🔹 Kubernetes

Uygulama Kubernetes üzerinde Deployment ve Service kullanılarak çalıştırılmıştır.

Deployment:

```text
deployment.yaml
```

Service:

```text
service.yaml
```

Temel Kubernetes komutları:

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get nodes
kubectl describe pod <pod-adı>
kubectl logs <pod-adı>
```

## 🔹 Helm

Kubernetes kaynaklarını paketlemek ve yönetmek için Helm kullanılmıştır.

Helm chart yapısı:

```text
helm/
├── Chart.yaml
├── values.yaml
├── helm-notes.md
└── templates/
    ├── deployment.yaml
    └── service.yaml
```

Helm ile uygulama kurulumu örneği:

```bash
helm install nginx-chart .
```

## 🔹 ArgoCD

ArgoCD kullanılarak GitOps yaklaşımı uygulanmıştır.

ArgoCD, GitHub repository'sindeki Kubernetes manifestlerini takip ederek uygulamanın Kubernetes cluster'ındaki durumunu yönetmektedir.

Akış:

```text
GitHub
   ↓
ArgoCD
   ↓
Kubernetes
   ↓
demo-app
```

ArgoCD uygulaması:

```text
Application: demo-app
Repository: https://github.com/sevinc0/demo.git
Branch: main
Path: .
Namespace: default
```

Uygulama ArgoCD üzerinde:

```text
Sync Status: Synced
Health Status: Healthy
```

durumuna getirilmiştir.

## 🔹 DevOps Genel Akışı

Projede öğrenilen araçların genel kullanım amacı:

```text
Git / GitHub
      ↓
   Jenkins
      ↓
    Maven
      ↓
    Docker
      ↓
 Kubernetes
      ↑
    ArgoCD
      ↑
   GitHub
```

### Araçların Görevleri

| Araç         | Kullanım Amacı                            |
| ------------ | ----------------------------------------- |
| Git / GitHub | Kaynak kod yönetimi                       |
| Maven        | Java projesi build ve dependency yönetimi |
| Jenkins      | CI/CD pipeline                            |
| Docker       | Container ve image yönetimi               |
| Kubernetes   | Container orchestration                   |
| Helm         | Kubernetes package management             |
| ArgoCD       | GitOps ve continuous delivery             |
| PostgreSQL   | Veritabanı                                |


## 📚 Öğrenilen Konular

Bu proje kapsamında aşağıdaki DevOps konuları üzerinde çalışılmıştır:

* Git ve GitHub
* Git branch ve Pull Request mantığı
* Maven
* Jenkins Pipeline
* Jenkinsfile
* Docker image ve container
* Docker network
* Docker Compose
* PostgreSQL container
* Kubernetes Pod
* Kubernetes Deployment
* Kubernetes Service
* Helm Chart
* ArgoCD
* GitOps yaklaşımı
* CI/CD pipeline
* Health Check
* Container ve Kubernetes troubleshooting

## 🎯 Sonuç

Bu proje ile basit bir Spring Boot uygulamasının kaynak koddan başlayarak build edilmesi, Docker image oluşturulması, container olarak çalıştırılması, Kubernetes üzerinde deploy edilmesi ve ArgoCD ile GitOps yaklaşımı kullanılarak yönetilmesi uygulamalı olarak gerçekleştirilmiştir.

Projenin amacı production seviyesinde karmaşık bir sistem kurmak yerine DevOps araçlarının temel çalışma mantığını uygulamalı olarak öğrenmektir.
