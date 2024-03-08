## Skaffold

Skaffold hydrates your Kubernetes manifest with the image you built and tagged, and deploys the application.

Docker: Builds and manages containerized applications by packaging code and dependencies into lightweight, portable containers.

Kubernetes: Orchestrates and manages the deployment, scaling, and maintenance of containerized applications across a cluster of nodes.

Skaffold: Automates the workflow of building, pushing, and deploying Kubernetes-native applications, enabling efficient development and deployment of containerized applications.

First, let's start with the project structure:

```
my-app/
├── backend/
│   ├── src/
│   ├── Dockerfile
│   └── ...
├── frontend/
│   ├── src/
│   ├── Dockerfile
│   └── ...
├── k8s/
│   ├── backend.yaml
│   ├── frontend.yaml
│   └── ...
└── skaffold.yaml
```

In this structure, the `k8s` folder contains the Kubernetes manifests (e.g., `backend.yaml` and `frontend.yaml`) that define the deployments, services, and other resources required to run the application on a Kubernetes cluster.

Now, let's discuss the development workflow without Skaffold (manual process):

1. Make changes to the source code (e.g., `backend/src/main.go` or `frontend/src/App.js`).
2. Navigate to the respective component directory (e.g., `cd backend`).
3. Build the Docker image: `docker build -t my-backend:v1 .`
4. Push the Docker image to a registry: `docker push my-backend:v1`
5. Update the corresponding Kubernetes manifest in the `k8s` folder (e.g., `k8s/backend.yaml`) with the new image tag.
6. Apply the updated manifest: `kubectl apply -f k8s/backend.yaml`
7. Repeat steps 2-6 for any other components that were changed.

As you can see, this manual process is tedious and time-consuming, especially when dealing with multiple components or making frequent changes during development.

Now, let's look at the workflow with Skaffold:

1. Create a `skaffold.yaml` file in the project root directory (`my-app/`). This file defines the build and deployment configurations for your application components. Here's an example:

   ```yaml
   apiVersion: skaffold/v2beta22
   kind: Config
   build:
     artifacts:
     - image: my-backend
       context: backend
     - image: my-frontend
       context: frontend
   deploy:
     kubectl:
       manifests:
       - k8s/*.yaml
   ```

   In this configuration, Skaffold is instructed to build two container images (`my-backend` and `my-frontend`) from the `backend` and `frontend` directories, respectively. It also specifies that Skaffold should deploy the application using the Kubernetes manifests in the `k8s/` directory.

2. Start Skaffold in development mode: `skaffold dev`
3. Skaffold will build and deploy your application components initially.
4. Make changes to your source code (e.g., `backend/src` or `frontend/src`).
5. Skaffold detects the file changes and automatically rebuilds the affected container images and redeploys the updated components using the manifests in the `k8s` folder.
6. You can continue making changes, and Skaffold will keep rebuilding and redeploying as needed.

By using Skaffold, you no longer need to manually build, tag, push, and redeploy your containers every time you make a change. Skaffold automates this process, allowing you to focus on writing code and seeing your changes reflected in the running application almost immediately.

The key advantages of using Skaffold are:

- **Automated build and deployment**: Skaffold handles the building, tagging, pushing, and redeploying of container images automatically, saving you significant time and effort.
- **File watching**: Skaffold watches for file changes in your project directories and triggers the necessary rebuild and redeploy actions.
- **Consistent deployment**: Skaffold uses the Kubernetes manifests in the `k8s` folder to ensure consistent deployment across different environments (e.g., development, staging, production).
- **Efficient workflow**: By automating the tedious tasks, Skaffold streamlines the development workflow, allowing you to be more productive and focused on writing code.

Overall, Skaffold is a powerful tool that simplifies the development process for containerized applications, especially those deployed on Kubernetes clusters. It addresses the shortcomings of the manual process by automating the build, push, and deployment steps, while also providing file watching capabilities to ensure your changes are reflected in the running application quickly.