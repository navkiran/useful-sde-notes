## Steps with Kubernetes yamls

```
my-app/
├── Dockerfile
├── deployment.yaml
├── service.yaml
├── src/
│   ├── index.js
│   ├── package.json
│   └── ...
```

1. **Build Docker Image**
   - Create a `Dockerfile` for your application
   - Build the Docker image using `docker build -t my-app:v1 .`

2. **Push Image to Registry**
   - Push the Docker image to a registry: `docker push my-registry/my-app:v1`

3. **Create Kubernetes Deployment YAML**
   - Define the Deployment in `deployment.yaml`

4. **Create Kubernetes Service YAML**
   - Define a Service in `service.yaml`

5. **Apply Kubernetes YAMLs**
   - Run `kubectl apply -f deployment.yaml` to create the Deployment
   - Run `kubectl apply -f service.yaml` to create the Service

6. **Verify Deployment**
   - Run `kubectl get pods` and `kubectl get services`
   - Access the application using the service endpoint

7. **Update Application**
   - Build a new Docker image with updated code
   - Push the new image to the registry
   - Update the image tag in `deployment.yaml`
   - Run `kubectl apply -f deployment.yaml` to update the Deployment

8. **Scale Application**
   - Update the `replicas` field in `deployment.yaml`
   - Run `kubectl apply -f deployment.yaml` to update the Deployment

9. **Rollback Application**
   - Run `kubectl rollout undo deployment/my-app` to rollback to the previous revision

## Using Helm Instead

Sure, here's the directory structure when using Helm to containerize an application:

```
my-app/
├── Dockerfile
├── backend/
│   ├── package.json
│   ├── server.js
│   └── ...
├── frontend/
│   ├── package.json
│   ├── src/
│   │   ├── App.js
│   │   └── ...
│   └── public/
│       ├── index.html
│       └── ...
└── helm/
    └── my-app/
        ├── Chart.yaml
        ├── values.yaml
        └── templates/
            ├── deployment.yaml
            ├── service.yaml
            └── ...
```

Here's what each directory and file represents:

- `Dockerfile`: Contains the instructions to build the Docker image for your application.
- `backend/`: Contains the source code for the Node.js backend.
- `frontend/`: Contains the source code for the React frontend.
- `helm/my-app/`: This directory contains the Helm chart for your application.
  - `Chart.yaml`: Metadata file for the Helm chart, including the chart name, version, and description.
  - `values.yaml`: Configuration file where you can define values for your application, such as container image, replicas, service ports, etc.
  - `templates/`: Directory containing the Kubernetes resource templates for your application.
    - `deployment.yaml`: Template for the Kubernetes Deployment resource.
    - `service.yaml`: Template for the Kubernetes Service resource.
    - Other templates can be added as needed, such as ConfigMaps, Secrets, Ingress, etc.

When using Helm, you define the desired state of your application in the `templates/` directory, using Go templating syntax to reference values from the `values.yaml` file. This allows you to easily configure and manage your application's deployment and resources using a single command.

For example, in the `templates/deployment.yaml` file, you might have the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}-app
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}-app
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        ports:
        - containerPort: {{ .Values.service.port }}
```

In this example, the values defined in `values.yaml` (e.g., `replicaCount`, `image.repository`, `image.tag`, `service.port`) are used to populate the Deployment template.

By using Helm, you can package your application along with its dependencies and deploy it with a single command, making it easier to manage and maintain your Kubernetes applications.
