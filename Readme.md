# Helm Kubernetes Demo

This project demonstrates deploying a simple nginx application on a Kubernetes cluster using Helm. The provided Helm chart simplifies the deployment by centralizing configuration in the values.yaml file, allowing for easy customization. By modifying this file, you can adjust the deployment and service settings or adapt the chart to deploy a different application without modifying the core templates.

Helm's templating system ensures that all configuration variables are maintained in a single location, making the chart flexible and easy to manage for various use cases.

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/) Or  any other Kubernetes cluster setup internal or external.
- [Helm](https://helm.sh/docs/intro/install/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-docker/)

## Setup and Deployment

1. Create a secret for the docker registry
```
kubectl create secret docker-registry default-secret \
  --docker-username=<your-docker-username> \
  --docker-password=<your-docker-password> \
  --docker-email=<your-docker-email> \
  --namespace=<your-namespace>
```
2. **Start Minikube:**
   ```sh
   minikube start
3. Run the command
   ```sh
   helm upgrade --install helm-kubernetes-demo ./helm 
   ```

4. Run the command to get the external IP of the service.
    ```sh
    kubectl get svc
   ``` 
   
5. Open the browser and navigate to the external IP of the service to view the application.


## Chart Structure
This Helm chart contains the following:

- **Deployment:** Manages the application pods.
- **Service:** Exposes the application inside the cluster (or externally depending on configuration).
- **Ingress:** Exposes the application to the outside world.
- **namespace:** Creates a namespace for the application.
### Files
- ```Chart.yaml```: Metadata about the Helm chart.
- ```values.yaml```: Default configuration values for the chart (e.g., service type, ports, image information) 
- ```./templates```: Contains Kubernetes YAML templates for resources like Deployment and Service.

**Note:** The templates use Go templating to insert values from the values.yaml file into the YAML files.

### Important: 
Do not commit the values.yaml file with sensitive information like passwords or API keys. 
Use Kubernetes Secrets or external secret management toolsd
to store and access sensitive data.


## Verify Installation
To verify the installation:

- Check the pods and services:

```sh
kubectl get pods -n <namespace>
kubectl get svc
```
Get details about the release:

```sh
helm list
helm status <my-app-name>
```
## Uninstalling the Chart

To uninstall the my-app release, run the following command:
    
```sh
helm uninstall <my-app-name>
```
This will delete all Kubernetes resources associated with the release.

## Troubleshooting
Ensure that your kubectl is configured correctly for your cluster.
Use helm get manifest my-app to view the resources generated by the chart.
Check pod logs for errors:
```sh
kubectl get pods  -n <namespace>
kubectl get <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace>
```
## License
This project is licensed under the MIT License.