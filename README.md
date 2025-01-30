# End-to-End DevOps Deployment

This project demonstrates a complete DevOps pipeline for deploying a microservices-based application using containerization, CI/CD, infrastructure as code, Kubernetes, and monitoring tools.

## Features

- **Containerization:** Microservices are containerized using Docker.
- **CI/CD Pipeline:** Automated delivery using GitHub Actions.
- **Infrastructure as Code (IaC):** Provisioning and configuration management using Terraform.
- **Kubernetes Orchestration:** Managing containerized applications on Kubernetes.
- **Cloud Integration:** Managed Kubernetes Service and other cloud resources.
- **Monitoring and Logging:** Grafana and Prometheus for observability.


## Project Structure

```
.
├── expensy_frontend/                # Frontend files and Docker configuration
│   ├── Dockerfile                   # Dockerfile to build the frontend container
│   ├── (other frontend files)      # All other necessary files (JS, CSS, HTML, etc.)
├── expensy_backend/                 # Backend files and Docker configuration
│   ├── Dockerfile                   # Dockerfile to build the backend container
│   ├── (other backend files)       # All other necessary files (Node.js, Python, etc.)
├── docker-compose.yaml              # Docker Compose for local development
├── kubernetes/                      # Kubernetes configurations
│   ├── mongodb.yaml                # MongoDB configuration (PV, PVC, Deployment, Service)
│   ├── redis.yaml                  # Redis configuration (PV, PVC, Deployment, Service)
│   ├── backend.yaml                # Backend configuration (Deployment, Service)
│   ├── frontend.yaml               # Frontend configuration (Deployment, Service)
├── terraform/                       # Terraform configuration for AWS EKS setup
│   ├── eks/                         # EKS-specific configurations
│   │   ├── variable.tf             # Terraform variables
│   │   ├── versions.tf             # Required provider versions
│   │   ├── security-groups.tf     # Security group configurations for EKS
│   │   ├── eks-cluster.tf         # EKS cluster definition
│   │   ├── vpc.tf                 # VPC definition for networking
│   │   ├── outputs.tf             # Outputs like EKS cluster info, VPC, etc.
├── .github/                         # GitHub Actions for CI/CD pipeline
│   ├── workflows/                   # CI/CD workflows
│   │   ├── ci-cd.yaml              # GitHub Actions YAML file for CI/CD pipeline
└── README.md                        # Project documentation

```

---

## 1. Containerizing the Microservices

### After providing the `dockerfiles` and `docker-compose.yaml`  file, Run with Docker Compose:

```sh
docker-compose up -d
```

---

## 2. CI/CD Pipeline with GitHub Actions

The pipeline is defined in `.github/workflows/ci-cd.yaml`. It automates building, testing, and deployment.

---

## 3. Infrastructure Management with Terraform

Terraform files define the AWS EKS cluster and networking.

### **Initialize and Apply Terraform**

```sh
cd terraform-eks
terraform init
terraform plan
terraform apply -auto-approve
```

Terraform provisions:

- **VPC** (`vpc.tf`)
- **Security Groups** (`security-groups.tf`)
- **EKS Cluster** (`eks-cluster.tf`)

---

## 4. Deploying to Kubernetes

In this step to apply all the manifests, will be done automatically  through `ci-cd.yaml` on AWS EKS

```sh
kubectl apply -f kubernetes/
```

Key files:

- **Persistent Volumes & Claims:** `mongodb-pv.yaml`, `mongodb-pvc.yaml`, `redis-pv.yaml`, `redis-pvc.yaml`
- **Configs & Secrets:** `app-config.yaml`, `app-secrets.yaml`
- **Microservices Deployments and Services:** `backend.yaml`, `frontend.yaml`
- **Databases Deployments and Services:** `mongodb.yaml`, `redis.yaml`

---

## 5. Monitoring Kubernetes Cluster and its objects with Grafana & Prometheus

- Create a Namespace for Prometheus:\
  `kubectl create namespace prometheus` 

- Install Prometheus and Grafana:\
  `helm install stable prometheus-community/kube-prometheus-stack -n prometheus` 

- Check the pods running in the Prometheus namespace to ensure Prometheus and Grafana are deployed:\
  `kubectl get pods -n prometheus` 

- Check the services running in the Prometheus namespace to confirm Prometheus and Grafana services are created:\
  `kubectl get svc -n prometheus` 

- Change the Prometheus service type to LoadBalancer:\
  `kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus` 

- Change the Grafana service type to LoadBalancer:\
  `kubectl edit svc stable-grafana -n prometheus` 

- Check the services again to verify that they are now using LoadBalancer and get the external IP (URL) for both Prometheus and Grafana:\
  `kubectl get svc -n prometheus` 

- Access the Grafana Dashboard\
  Access the Grafana UI in your browser using the external URL you obtained in the previous step.

  Username: admin

  Password: prom-operator

  Open the URL in your browser and log in with the provided credentials.

---

## Conclusion

This project implements an end-to-end DevOps pipeline, integrating containerization, CI/CD automation, cloud deployment, Kubernetes orchestration, and monitoring. Modify configurations to fit your requirements and deploy seamlessly.

