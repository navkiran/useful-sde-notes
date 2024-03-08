# Kubectl Commands

## List Resources
kubectl get <resource-type> [options]
# e.g., kubectl get pods, kubectl get services

## Describe a Resource
kubectl describe <resource-type> <resource-name>
# e.g., kubectl describe pod my-pod

## Search Pods Starting with a Name
kubectl get pods --selector=app=<name-prefix>
# e.g., kubectl get pods --selector=app=nginx

## Delete Pods with ContainerStatusUnknown
kubectl get pods --field-selector=status.phase!=Running --output=name | xargs kubectl delete --force --grace-period=0

## Port Forwarding
kubectl port-forward <pod-name> <local-port>:<remote-port>
# e.g., kubectl port-forward my-pod 8080:80

## Logs
kubectl logs <pod-name> [-c <container-name>]
# e.g., kubectl logs my-pod, kubectl logs my-pod -c nginx

## Exec into a Container
kubectl exec -it <pod-name> -- <command>
# e.g., kubectl exec -it my-pod -- /bin/bash

# Docker Commands

## Image Prune
docker image prune [options]
# Remove unused images

## Compose
docker-compose up [-d] [--build]
# Start containers defined in docker-compose.yml
# -d: Detached mode (runs in background)
# --build: Build images before starting containers

docker-compose down
# Stop and remove containers

## Build
docker build -t <image-name>:<tag> <path>
# Build a Docker image from a Dockerfile
# e.g., docker build -t my-app:v1 .

## Run
docker run [options] <image-name>:<tag> [command]
# Run a Docker container
# e.g., docker run -d -p 8080:80 my-app:v1
