
### 📄 `README.md`

markdown

CopyEdit

``# crud-api 🛠️ A minimal, secure, and fully portable CRUD API built for DevSecOps demos.  
Works without a database, using in-memory storage. Deployed via Docker and k3s inside a Multipass VM.

--- ## �� Tech Stack  - Python Flask - Prometheus client for `/metrics`  - Docker - Kubernetes (k3s via Multipass) - Port-forwarding to host for local testing

--- ## 📁 Project Structure`` 

crud-api/  
├── app.py # Flask app with full CRUD and metrics  
├── Dockerfile  
├── requirements.txt  
├── ci.sh # Build + deploy + port-forward  
└── k8s/  
├── deployment.yaml  
└── service.yaml

yaml

CopyEdit

 `--- ## 🚀 Setup Instructions (Inside Ubuntu Multipass VM)  ### Step 1: Shell into your devvm  ```bash  multipass  shell  devvm` 

### Step 2: Build and deploy



`cd ~/quicknote-api
./ci.sh` 

This script will:

-   Build the Docker image
    
-   Apply K8s deployment and service YAMLs
    
-   Port-forward service from `localhost:8081` → app port `5000`
    

> ⚠️ If port 8081 is taken, edit `ci.sh` to use another available port.

----------

## 🔐 Authentication

All `POST`, `PUT`, and `DELETE` requests require a header:



`X-Key: demo123` 

----------

## 🧪 Example `curl` Commands

### Create a note



`curl -X POST -H "X-Key: demo123" -H "Content-Type: application/json" \
-d '{"note_id":"n1", "text":"Initial note"}' \
http://localhost:8081/notes` 

### Get all notes

bash

CopyEdit

`curl http://localhost:8081/notes` 

### Get a single note

bash

CopyEdit

`curl http://localhost:8081/notes/n1` 

### Update a note

bash

CopyEdit

`curl -X PUT -H "X-Key: demo123" -H "Content-Type: application/json" \
-d '{"text":"Updated content"}' \
http://localhost:8081/notes/n1` 

### Delete a note

bash

CopyEdit

`curl -X DELETE -H "X-Key: demo123" http://localhost:8081/notes/n1` 

----------

## 📈 Observability Endpoints

-   Health Check:  
    `GET /health` → `{ "status": "ok" }`
    
-   Prometheus Metrics:  
    `GET /metrics` → exposes `http_requests_total` counter
    

----------

## 🧠 Adaptability

You can quickly change this project into:

Use Case

Rename Route to

Note Fields Become...

Secrets Manager

`/secrets`

`secret_id`, `value`

Task Tracker

`/tasks`

`task_id`, `description`

Config Store

`/configs`

`config_key`, `config_val`

Policy Store

`/policies`

`policy_id`, `yaml_blob`

----------

## 🧼 Cleanup

To delete the app:

bash

CopyEdit

`kubectl delete deployment crud-api
kubectl delete svc crud-api` 

----------

## 🧷 Notes

-   This is stateless. Data is stored in RAM and lost on container restart.
    
-   For persistence, you can integrate Redis or SQLite.
    
-   Designed for secure offline demos without cloud dependencies.
    

----------

## 🔒 Auth, Infra, Observability — All in One

✅ Secure headers  
✅ K8s Secrets-ready  
✅ Prometheus metrics  
✅ Health endpoint  
✅ No DB dependency  
✅ <3-minute deploy from scratch

