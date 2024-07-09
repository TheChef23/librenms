# Kubernetes

This guide walks you through the process of deploying LibreNMS on a Kubernetes cluster using Helm charts. It is designed for beginners and assumes basic knowledge of Kubernetes and Helm.

## Prerequisites

Before you begin, ensure you have the following:

* A running Kubernetes cluster.
* `kubectl` command-line tool installed and configured to interact with your cluster.
* `helm` command-line tool installed.

## Steps to Deploy LibreNMS

### 1. Add the Helm Repository

First, add the LibreNMS Helm chart repository to your Helm setup.

```bash
helm repo add librenms https://librenms.github.io/helm-charts/
helm repo update
```

### 2. Create a Namespace

Create a namespace for LibreNMS to keep your deployment isolated.

```bash
kubectl create namespace librenms
```

### 3. Install the Chart

Install the LibreNMS Helm chart. You can customize the installation by specifying values. For a basic installation, you can use the default values.

```bash
helm install librenms librenms/librenms --namespace librenms
```

### 4. Configure the Deployment

You can customize the deployment using a `values.yaml` file. Create a file named `values.yaml` and specify your desired configuration. Here is an example configuration:

```yaml
replicaCount: 1
image:
  repository: librenms/librenms
  tag: "latest"
  pullPolicy: IfNotPresent
```

Then, apply the customized configuration during the installation:

```bash
helm install librenms librenms/librenms --namespace librenms -f values.yaml
```

### 5. Verify the Deployment

Check the status of the deployment to ensure everything is running correctly.

```bash
kubectl get pods -n librenms
```

You should see the LibreNMS pods running in your namespace.

### 6. Access LibreNMS

To access the LibreNMS web interface, you need to expose the service. You can use a `NodePort`, `LoadBalancer`, or an Ingress resource. Here is an example of how to create a basic `NodePort` service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: librenms
  namespace: librenms
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30000
  selector:
    app: librenms
```

Apply the service definition:

```bash
kubectl apply -f service.yaml
```

Now, you can access LibreNMS by navigating to the IP address of any node in your cluster on port `30000`.

### 7. Cleanup

If you need to uninstall LibreNMS, you can do so using the following command:

```bash
helm uninstall librenms --namespace librenms
```

Additionally, delete the namespace if you no longer need it:

```bash
kubectl delete namespace librenms
```

---

This completes the step-by-step guide for deploying LibreNMS on Kubernetes using Helm charts. For more advanced configurations and options, refer to the official LibreNMS Helm chart [documentation](https://github.com/librenms/helm-charts/tree/develop/charts/librenms).

If you encounter any issues or have questions, feel free to ask the LibreNMS community for help. Happy monitoring!