# Containerization with Kubernetes





## What is Kubernetes?

### Orchestrator Features

* Provision(供应；提供)hosts
* Instantiate containers on a host
* Restart failing containers
* Expose containers as services outside the cluster



### Kubernetes (K8s)

* **Definition**: an open source platform designed to `automate deploying,` `scaling`, and `operating` application containers
* **Goal**: to foster an ecosystem of components and tools that relieve the burden of running application in public and private clouds



**Kubernetes** is a platform to schedule and run `containers` on clusters of virtual machine.

It runs on bare metal, virtual machines, private datacenter and public cloud.



### Kubernetes and Docker

* Kubernetes is a **container platform**.
* You can use `Docker` containers to develop and build applications, and then use `Kubernetes` to run these applications on your infrastructure.

rkt 也可用



## Kubernetes features

### Multi-Host Container Scheduling

* Done by the kube-scheduler
* Assigns pods to nodes at runtime
* Checks resources, quality of service, policies, and user specifications before scheduling

### Scalability and Availability

* Kubernetes master can be deployed in a highly available configuration
* Multi-region deployments available

> **Pods** are the smallest, most basic deployable objects in **Kubernetes**. A **Pod** represents a single instance of a running process in your cluster. **Pods** contain one or more containers, such as `Docker` containers. When a **Pod** runs multiple containers, the containers are managed as a single entity and share the **Pod's** resources.





