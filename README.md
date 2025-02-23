# K8s-project

## Learning notes 🤓

Kubernetes is a container orchestration platform that coordinates the collaboration of **Master Nodes** and **Worker Nodes**.

Master Nodes (Control Plane) are responsible for scheduling and deciding where the applications run. 

Worker Nodes provide the infrastructure to run the applications.

______

## Pods

In K8s, pods are the smallest deployable units and encapsulate application containers.
Every pod is given a virtual IP address, and as they are ephemeral their IP address changes whenever they are created and destroyed.
So IP addresses are not a very reliable way to access the application.

![](/diagrams/pod.drawio.svg)

#### Pod Configuration

  1. **Metadata**
      - `name` : Uniquely identifies the pod

      - `labels`: Categorize pods into distinct groups for flexible querying

  2. **Runtime requirements**
      - Container name

      - Image source

      - Port for serving requests

      - Resource requirements (CPU and memory)
        - Memory limit and requests should be the same
        - CPU limit should rarely be set (as per K8s best practices)

#### Multi-Container Pod

Pods can run multiple containers, enabling sidecar patterns where auxiliary sidecar containers can communicate with the main application container via localhost.

#### Port Forwarding in K8s

Port forwarding creates a temporary connection between your local machine and a pod in the cluster. It's primarily used for debugging and testing processes.

The command structure is: `kubectl port-forward <pod-name> <local-port>:<pod-port>`.

For example, `kubectl port-forward mypod 8080:80` forwards local port 8080 to port 80 of the pod.

_____

## Service

The Service in K8s abstracts away the network complexity and provides a durable endpoint for accessing pods.

#### NodePort Service
Allows external access to the K8s network by exposing a static port on the node (range: `3000-32767`).

```
apiVersion: v1
kind: Service
metadata: 
  name: grade-submission-portal
spec:
  type: NodePort
  selector:
    app: grade-submission-portal
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080
```

- The external request is initiated on the node's static port (**nodePort: 30080**).

- The request enters the cluster through the Service's internal port (**port: 8080**). With NodePort services, this internal port can be any valid port number, as the external nodePort(30080) will be mapped to whatever internal port is specified.

- The service acts as a proxy directing request to a matching pod using a label selector.

- The service specifies a target port (**targetPort: 8080**) that ensures the request reaches the container port on the pod.

The NodePort service is often used for prototyping, rarely in practice.

#### ClusterIP Service
Used for internal pod-to-pod communication within the cluster.

```
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: ClusterIP
  selector:
    app: backend
  ports:
    - port: 8080
      targetPort: 8080
```

- Pods within the cluster can access the service using the service name (`backend-service`) and service's internal port (`port: 8080`).

## Namespace
A logical concept to group resources that are closely related, invisible boundaries created by the ETCD database to make easy for developers to manage close resources.

You can't access the cluster directly, you are given access to certain namespaces based on your role.