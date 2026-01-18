# Kubernetes Demo Project: Web App with MongoDB

This project demonstrates how to deploy a full-stack application (Node.js Web App + MongoDB) onto a Kubernetes cluster (Azure AKS). It utilizes core Kubernetes components including Deployments, Services, ConfigMaps, and Secrets to manage state and configuration securely.

## 📋 Project Overview

The application is a User Profile demo.
* **Frontend/Backend:** A Node.js web application that serves a UI and connects to the database.
* **Database:** MongoDB instance for persistent storage of user data.
* **Infrastructure:** Kubernetes (AKS) handling orchestration, networking, and secret management.

## 🛠 Prerequisites

* **Azure CLI** (for creating the cluster)
* **kubectl** (configured to talk to your Azure cluster)
* **Docker** (optional, for local testing)

## 📂 Project Structure

```text
k8s-config/
├── mongo-config.yaml    # ConfigMap: Stores the DB URL (non-sensitive)
├── mongo-secret.yaml    # Secret: Stores DB User & Password (Base64 encoded)
├── mongo.yaml           # Deployment & Service for MongoDB
├── webapp.yaml          # Deployment & Service for the Web Application
└── README.md            # Project documentation

```

## 🚀 Deployment Instructions

### 1. Configure Secrets

> **Note:** The `mongo-secret.yaml` file contains sensitive credentials and is usually ignored by git. For this demo, ensure the file exists with the following keys:

* `mongo-user` (Base64 encoded username)
* `mongo-password` (Base64 encoded password)

### 2. Apply Configuration

Navigate to the `k8s-config` folder and apply all resources:

```bash
cd k8s-config
kubectl apply -f .

```

### 3. Verify Deployment

Check that pods are running and services are created:

```bash
kubectl get all

```

### 4. Access the Application

The web application service is configured as a `LoadBalancer`. Retrieve the external IP address:

```bash
kubectl get service webapp-service --watch

```

Copy the `EXTERNAL-IP` and open it in your browser (e.g., `http://20.55.122.10`).

## ⚙️ Configuration Details

| Component | Key | Value / Source |
| --- | --- | --- |
| **Web App Image** | `image` | `nanajanashia/k8s-demo-app:v1.0` |
| **DB Connection** | `DB_URL` | From `mongo-config` (ConfigMap) |
| **DB Auth** | `USER_NAME` | From `mongo-secret` (Secret) |
| **DB Auth** | `USER_PWD` | From `mongo-secret` (Secret) |


## 🧹 Clean Up

To avoid Azure charges, delete the resource group after finishing the demo:

```bash
az group delete --name my-k8s-project --yes --no-wait

```
