# Private Chat AI Deployment

This repository contains Kubernetes manifests to deploy **Ollama** (backend service) and **Open Web UI**, providing a private AI chatbot. The setup includes Deployments and Services to ensure automated initialization, seamless internal communication, and external accessibility.

## Files Overview

1. **`backend-deployment.yaml`**
   - Defines the Deployment for **Ollama**, running a specific AI model (`llama3.2:1b`).
   - Includes a lifecycle hook to automate AI model initialization after pod creation.

2. **`backend-svc.yaml`**
   - Defines a **ClusterIP Service** for Ollama to ensure internal communication within the cluster.

3. **`webui-deployment.yaml`**
   - Defines the Deployment for **Open Web UI**, which serves as the user interface for the chatbot.
   - Configures an environment variable (`OLLAMA_BASE_URL`) to connect to the Ollama backend.

4. **`webui-svc.yaml`**
   - Defines a **LoadBalancer Service** to expose the Open Web UI to external users.

---

## Deployment Steps

### 1. Deploy Ollama (Backend Service)
1. Apply the Ollama Deployment:
   ```bash
   kubectl apply -f backend-deployment.yaml
   ```
   - This Deployment automatically starts the AI model using a lifecycle hook.

2. Apply the Ollama Service:
   ```bash
   kubectl apply -f backend-svc.yaml
   ```
   - The ClusterIP Service exposes Ollama internally at `backend-svc.jay.svc.cluster.local`.

### 2. Deploy Open Web UI (Frontend)
1. Apply the Open Web UI Deployment:
   ```bash
   kubectl apply -f webui-deployment.yaml
   ```
   - The Deployment sets up the UI and connects it to the backend service.

2. Apply the Open Web UI Service:
   ```bash
   kubectl apply -f webui-svc.yaml
   ```
   - The LoadBalancer Service assigns an external IP, making the UI accessible to users.

---

## Accessing the Open Web UI
1. Retrieve the external IP for the Open Web UI:
   ```bash
   kubectl get svc webui-svc -n jay
   ```
   - Note the `EXTERNAL-IP` field in the output.

2. Open the URL in your browser:
   ```
   http://<EXTERNAL-IP>
   ```

3. Register as a new user and start using your private chatbot!

---

## Key Features
- **Automated Initialization**: Ollama's Deployment lifecycle ensures the AI model starts without manual intervention.
- **Internal and External Communication**: 
  - Ollama (backend) is private, accessible only within the cluster.
  - Open Web UI (frontend) is publicly accessible via a LoadBalancer.
- **Seamless Integration**: The UI is pre-configured to interact with the backend service.

---

## References
- [Ollama](https://github.com/ollama/ollama)  
- [Open Web UI](https://github.com/open-webui/open-webui)  

Feel free to adapt the manifests and configurations to your specific needs!
