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
        - Ai-service: [GitHub Link](https://github.com/arishkakadiya/ai-service)
        - Virtual-customer: [GitHub Link](https://github.com/arishkakadiya/virtual-customer)
        - Virtual-worker: [GitHub Link](https://github.com/arishkakadiya/virtual-worker)
        - RabbitMQ: [GitHub Link](https://github.com/arishkakadiya/rabbitmq-service)
        - MongoDB: [GitHub Link](https://github.com/arishkakadiya/mongo-service)

2. **Docker Images**
    - Created Docker images for all services and uploaded them to Docker Hub:
        - Store-Front: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/store-front)
        - Order-Service: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/order-service)
        - Product-Service: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/product-service)
        - Makeline-Service: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/makeline-service)
        - AI-Service: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/ai-service)
        - Virtual-customer: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/virtual-customer)
        - Virtual-worker: [Docker Hub Link](https://hub.docker.com/repository/docker/arishkakadiya/virtual-worker)

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
2. **Get API Keys**:
   - Go to the **Keys and Endpoints** section of your Azure OpenAI resource.
   - Copy the **API Key (API key 1)** and **Endpoint URL**.

3. **Base64 Encode the API Key**:
   - Use the following command to Base64 encode your API key:
     ```bash
     echo -n "<your-api-key>" | base64
     ```
   - Replace `<your-api-key>` with your actual API key.

4. Configure deployment details in `secrets.yaml` and `ai-service.yaml`

### Step 4: Deploy Config and Secrets for RabbitMQ and OpenAI
```bash
kubectl apply -f secrets.yaml
kubectl apply -f config-maps.yaml
 ```

 ### Step 5: Deploy the Application with following commands

```bash
kubectl apply -f order-service.yaml
kubectl apply -f product-service.yaml
kubectl apply -f makeline-service.yaml
kubectl apply -f MongoDB.yaml
kubectl apply -f RabbitMQ.yaml
kubectl apply -f store-front.yaml
kubectl apply -f store-admin.yaml
kubectl apply -f ai-service.yaml

 ```

 ### Step 6: Validate the Deployment
- Check Pods and Services:
   ```bash
   kubectl get pods
   kubectl get services
   ```
- Test Frontend Access:
   - Locate the external IPs for store-front and store-admin services:
   ```bash
   kubectl get services
   ```
   - Access the Store Front app at the external IP on port 80.
   - Access the Store Admin app at the external IP on port 80.


## Step 6: Deploy Virtual Customer and Worker
   ```bash
   kubectl apply -f admin-tasks.yaml
   ```
- Monitor Virtual Customer:
   ```bash
   kubectl logs -f deployment/virtual-customer
   ```
- Monitor Virtual Worker:
   ```bash
   kubectl logs -f deployment/virtual-worker
   ```

### Step 7: Explore Advanced Features
### AI-Generated Descriptions and Images:
- Use the AI Service for generating product descriptions and images.
- Ensure your OpenAI API key is correctly configured in the deployed secret.
### RabbitMQ Management:
- Access the RabbitMQ management UI:
   ```bash
   kubectl port-forward service/rabbitmq 15672:15672
   ```
   The kubectl port-forward command is used to forward a local port to a port on a Kubernetes resource (e.g., a Pod or Service). This allows you to access the application running in the cluster from your local machine without exposing it externally.


- Login with the default credentials (`username`/`password`).

### MongoDB Shell Access and Database Exploration
In this section, you will use the MongoDB shell to interact with the `orderdb` database, which stores order information for the Algonquin Pet Store application. Follow the steps below to connect to the MongoDB pod and explore its contents.

#### **1- Access the MongoDB Shell**
Run the following command to connect to the MongoDB shell inside the running MongoDB pod:
```bash
kubectl exec -it <mongodb-pod-name> -- mongo
```
Explanation: This command uses kubectl exec to open an interactive shell (-it) inside the MongoDB pod and starts the MongoDB shell program (mongo).

#### **2- List All Databases**
Once inside the MongoDB shell, run:
```bash
show dbs
```
Explanation: The show dbs command lists all databases available on the MongoDB server. You should see a list that includes the orderdb, which stores order-related data for the application.
#### **3- Switch to the Order Database**
```bash
use orderdb
```
Explanation: The use orderdb command selects the orderdb database, making it the active database for subsequent queries and commands.
#### **4- List Collections in the Database**
Display all collections in the orderdb database:
```bash
show collections
```
Explanation: The show collections command lists all collections (similar to tables in relational databases) in the current database. The orders collection contains the order data.
#### **5- Query the Orders Collection**
Retrieve all documents in the orders collection:
```bash
db.orders.find()
```