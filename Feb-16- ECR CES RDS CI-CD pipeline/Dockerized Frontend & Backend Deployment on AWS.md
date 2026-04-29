## **1. Project Overview**

This project demonstrates a complete DevOps workflow for deploying a containerized frontend and backend application on AWS. The workflow includes:

- Dockerizing frontend and backend applications
    
- Storing images in DockerHub and AWS ECR
    
- Creating a MySQL database using AWS RDS
    
- Deploying applications on AWS ECS using Fargate
    
- Setting up CI/CD pipeline using GitHub Actions
    

The architecture ensures scalability, maintainability, and automation for future development.

---

## **2. Architecture Diagram**

- Frontend interacts with users
    
- Backend provides API services
    
- Backend connects to RDS database
    
- Docker images stored in ECR for CI/CD automation
    

---

## **3. Docker Images**

**Steps Completed:**

- Built backend and frontend Docker images locally:
    

`docker build -t backend . docker build -t frontend .`

- Pushed images to DockerHub for temporary storage:
    

`docker tag backend:latest username/backend:latest docker push username/backend:latest docker tag frontend:latest username/frontend:latest docker push username/frontend:latest`

---

## **4. AWS RDS (MySQL) Setup**

**Purpose:** Backend database storage

**Steps:**

1. Login to AWS → RDS → Create Database
    
2. Selected engine: MySQL, Version: latest
    
3. DB settings:
    
    - Identifier: `my-backend-db`
        
    - Username: `admin`
        
    - Password: `StrongPassword123`
        
    - Storage: 20 GB
        
4. Connectivity:
    
    - VPC: default
        
    - Public access: Yes (for testing)
        
    - Security group: allow inbound MySQL (3306) temporarily
        
5. Copied endpoint and port for backend `.env` configuration
    

**Backend `.env` Example:**

`DB_HOST=my-backend-db.xxxxxx.ap-south-1.rds.amazonaws.com DB_USER=admin DB_PASSWORD=StrongPassword123 DB_NAME=testdb DB_PORT=3306`

---

## **5. AWS ECR Setup**

**Purpose:** Store Docker images for CI/CD deployment

**Steps:**

1. Go to AWS → ECR → Create repository: `backend-repo` and `frontend-repo`
    
2. Login to ECR:
    

`aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <account_id>.dkr.ecr.ap-south-1.amazonaws.com`

3. Tag images for ECR:
    

`docker tag backend:latest <account_id>.dkr.ecr.ap-south-1.amazonaws.com/backend-repo:latest docker tag frontend:latest <account_id>.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo:latest`

4. Push images to ECR:
    

`docker push <account_id>.dkr.ecr.ap-south-1.amazonaws.com/backend-repo:latest docker push <account_id>.dkr.ecr.ap-south-1.amazonaws.com/frontend-repo:latest`

---

## **6. AWS ECS Cluster Setup**

**Purpose:** Deploy containerized applications

**Steps:**

1. ECS → Create Cluster → Fargate launch type → Name: `my-project-cluster`
    

**Task Definitions:**

- Backend Task:
    
    - Image: backend ECR image
        
    - CPU: 0.5 vCPU, Memory: 1GB
        
    - Port mapping: 5000
        
    - Environment variables: DB_HOST, DB_USER, DB_PASSWORD, DB_NAME
        
- Frontend Task:
    
    - Image: frontend ECR image
        
    - Port mapping: 3000
        

**ECS Services:**

- Backend Service: Fargate, desired count: 1, mapped port 5000
    
- Frontend Service: Fargate, desired count: 1, mapped port 3000
    

**Networking:**

- VPC: default
    
- Subnets: 2 public subnets
    
- Auto-assign public IP: Enabled
    
- Security group: allow ports 3000 (frontend) & 5000 (backend)
    

---

## **7. CI/CD Pipeline (GitHub Actions)**

**Status:** Planned, not implemented due to AWS sandbox timeout

**Planned Workflow:**

- Trigger: `push` on `main` branch
    
- Steps:
    
    - Checkout repo
        
    - Configure AWS credentials (secrets in GitHub)
        
    - Login to ECR
        
    - Build Docker images
        
    - Tag & push to ECR
        
    - Update ECS services automatically
        

**Sample GitHub Actions File (`.github/workflows/deploy.yml`):**

`name: Deploy to ECS on:   push:     branches:       - main jobs:   deploy:     runs-on: ubuntu-latest     steps:       - uses: actions/checkout@v3       - uses: aws-actions/configure-aws-credentials@v2         with:           aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}           aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}           aws-region: ap-south-1       - uses: aws-actions/amazon-ecr-login@v1       - run: docker build -t backend .       - run: docker tag backend:latest <account_id>.dkr.ecr.ap-south-1.amazonaws.com/backend-repo:latest       - run: docker push <account_id>.dkr.ecr.ap-south-1.amazonaws.com/backend-repo:latest`