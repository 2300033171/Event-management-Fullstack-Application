# Event Management - Kubernetes Folder

This folder contains Dockerfiles and Kubernetes manifests for deploying:
- Frontend: Angular (served by nginx)
- Backend: Spring Boot (fat JAR)
- Database: MySQL (StatefulSet + PVC)

**Usage (example):**
1. Build images (replace <registry> and tags):
   - Frontend:
     ```
     cd frontend
     docker build -t <registry>/event-frontend:latest .
     ```
   - Backend:
     ```
     cd backend
     ./mvnw clean package -DskipTests
     docker build -t <registry>/event-backend:latest .
     ```
2. Push images to your registry.
3. Update `k8s/overlays/production/secret.yaml` and `k8s/overlays/production/configmap.yaml` with proper values.
4. Apply manifests:
   ```
   kubectl apply -f k8s/base/namespace.yaml
   kubectl apply -f k8s/base -n event-app
   kubectl apply -f k8s/overlays/production -n event-app
   ```
Notes:
- Ingress includes TLS placeholder annotations — replace with your cluster's ingress controller and certificate manager setup.
- For development/testing, you can use the `k8s/overlays/minimal` overlays.
