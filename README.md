# K8s-project

## Learning notes

Kubernetes is a container orchestration platform that coordinates the collaboration of **Master Nodes** and **Worker Nodes**.

Master Nodes (Control Plane) are responsible for scheduling and deciding where the applications run. 

Worker Nodes provide the infrastructure to run the applications.

______

### Pods

In K8s, pods are the smallest deployable units and encapsulate application containers.

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
