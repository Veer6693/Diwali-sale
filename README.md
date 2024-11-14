# Dagster Deployment

This guide walks you through the process of deploying Dagster on a Kubernetes cluster using Helm.

## Prerequisites

- Kubernetes cluster
- kubectl CLI tool
- Helm package manager

## Installation Steps

### 1. Create a Namespace for Dagster

Create a dedicated namespace for the Dagster deployment:

```bash
kubectl create namespace dagster
```

### 2. Add the Dagster Helm Repository

Add the Helm chart repository for Dagster and update it:

```bash
helm repo add dagster https://dagster-io.github.io/helm
helm repo update
```


### 3. Deploy Dagster Using Helm
Install Dagster into the dagster namespace with default configurations:

```bash
helm install dagster dagster/dagster -n dagster
```


### 4. Verify the Deployment
Check that all Dagster components are running properly:

```bash
kubectl get all -n dagster
```

### 5. Configure External Access
By default, Dagster's service uses ClusterIP and is only accessible within the cluster. To enable external access:

  1. First, identify the Dagster service:
  ```bash
  kubectl get svc -n dagster
  ```

  2. Edit the webserver service to use NodePort:
  ```bash
  kubectl edit svc dagster-dagster-webserver -n dagster
  ```

  3. In the editor, change the type field from `ClusterIP` to `NodePort`


### 6. Access the Dagster UI
Once configured, access the Dagster UI in your browser using:

```bash
http://<node-ip>:<node-port>
```

Replace <node-ip> with your node's IP address and <node-port> with the assigned NodePort.

![dagster-ui](https://github.com/user-attachments/assets/6287bfe3-03c8-4127-a3a1-0f7d0e2b2871)



### 7. Customize Configuration Using values.yaml
You can customize your Dagster deployment by modifying the configuration values. First, get the default values.yaml file:

```bash
helm show values dagster/dagster > values.yaml
```

After modifying the values.yaml file according to your requirements, upgrade the deployment:

```bash
helm upgrade dagster dagster/dagster -n dagster -f values.yaml
```
