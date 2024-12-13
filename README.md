# Best Buy Store (On Steroids) with RabbitMQ

As a Full-Stack Cloud-Native Developer at Best Buy, I have been assigned to develop a demo cloud-native application for the online store. The application will showcase a robust and scalable architecture, adhering to modern design principles inspired by the Algonquin Pet Store (On Steroids).

The application includes the following components:

## Application Overview

### Service Description

| Service         | Description                                                            | Notes                                      |
|------------------|------------------------------------------------------------------------|--------------------------------------------|
| **Store-Front** | Customer-facing app for browsing and placing orders.                  |                                            |
| **Store-Admin** | Employee-facing app for managing products and viewing orders.         |                                            |
| **Order-Service** | Handles order creation and sends data to the managed order queue.   | Uses RabbitMQ                              |
| **Product-Service** | Handles CRUD operations for product data.                         |                                            |
| **Makeline-Service** | Processes and completes orders by reading from the order queue.  |                                            |
| **AI-Service**  | Generates product descriptions and images.                            | Uses GPT-4 and DALL-E-3 models             |
| **Database**    | MongoDB for persisting order and product data.                        |                                            |

---

## Project Deliverables

1. **GitHub Repository**
    - Includes microservice repositories:
        - Store-Front: [GitHub Link](https://github.com/arishkakadiya/store-front)
        - Order-Service: [GitHub Link](https://github.com/arishkakadiya/order-service)
        - Product-Service: [GitHub Link](https://github.com/arishkakadiya/product-service)
        - Makeline-Service: [GitHub Link](https://github.com/arishkakadiya/makeline-service)
        - Store-Admin: [GitHub Link](https://github.com/arishkakadiya/store-admin)

2. **Docker Images**
    - Created Docker images for all services and uploaded them to Docker Hub:
        - Store-Front: [Docker Hub Link](https://hub.docker.com/r/klamichhane738/store-front-bestbuy/tags)
        - Order-Service: [Docker Hub Link](https://hub.docker.com/r/klamichhane738/order-service-bestbuy/tags)
        - Product-Service: [Docker Hub Link](https://hub.docker.com/r/klamichhane738/product-service-bestbuy/tags)
        - Makeline-Service: [Docker Hub Link](https://hub.docker.com/r/klamichhane738/makeline-service-bestbuy/tags)
        - AI-Service: [Docker Hub Link](https://hub.docker.com/r/klamichhane738/ai-service-bestbuy/tags)

3. **Deployment Files**
    - All Kubernetes deployment YAML files are included in the `Deployment-Files` folder.

---

## Implementation Steps

### Step 1: Clone the Repository
Clone the repository containing all necessary deployment files.

### Step 2: Set Up an AKS Cluster
Create an Azure Kubernetes Service (AKS) cluster with two worker nodes. Use the following configuration:
- **Node Pools**:
  - Master Pool: `D2as_v4`, 1 node
  - Worker Pool: `D2as_v4`, 2 nodes

### Step 3: Configure AI Backing Services
1. Create an Azure OpenAI resource to deploy GPT-4 and DALL-E-3 models.
2. Configure deployment details in `secrets.yaml`.

### Step 4: Deploy Config and Secrets
```bash
kubectl apply -f secrets.yaml
kubectl apply -f config-maps.yaml
