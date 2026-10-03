# Monolithic-Microservices-Containers-Docker

## 1. Monolithic Architecture

A **monolithic application** is an application where multiple business functionalities are developed, packaged, and deployed as a single application.

### Example — Banking Application

Consider a bank such as HDFC Bank. A banking platform may provide:

* Internet Banking
* Mobile Banking
* Loans
* Insurance
* Credit Cards
* Account Management
* Payments

In a monolithic architecture, many of these functionalities can be part of one large application.

```text
              Banking Application
        ┌─────────────────────────────┐
        │ Internet Banking            │
        │ Mobile Banking              │
        │ Loans                       │
        │ Insurance                   │
        │ Payments                    │
        │ Account Management          │
        └──────────────┬──────────────┘
                       ↓
                    Database
```

### Advantages

* Simple to develop initially
* Simple deployment
* Easier local development
* Less infrastructure complexity
* Easier to understand for small applications

### Disadvantages

* Large codebase as the application grows
* Entire application may need to be rebuilt and redeployed
* Scaling individual functionality can be difficult
* Tight coupling can develop between modules
* Maintenance becomes harder for very large applications

---

# 2. Microservices Architecture

**Microservices architecture** divides a large application into multiple smaller services.

Each service focuses on a specific **business capability** and can be developed, tested, deployed, and scaled independently where appropriate.

### Example — E-Commerce

An e-commerce platform can contain services such as:

```text
                   E-Commerce Platform
                           │
       ┌───────────────────┼───────────────────┐
       ↓                   ↓                   ↓
  User Service       Product Service      Order Service
       │                   │                   │
       ↓                   ↓                   ↓
    User DB            Product DB           Order DB

                           │
                ┌──────────┴──────────┐
                ↓                     ↓
        Payment Service       Notification Service
                │                     │
                ↓                     ↓
           Payment DB           Message System
```

### Important Characteristics

* Small, focused services
* Independent deployment
* Independent scaling
* Service-to-service communication
* Loose coupling
* Service-owned data is commonly preferred
* Automation and monitoring are important

> Note: A real platform such as Amazon contains many products and services, but individual customer-facing products such as Amazon Fresh, Prime, and others should not automatically be assumed to be separate microservices. Microservices are an architectural design choice inside a system.

---

# 3. Monolithic vs Microservices

| Feature          | Monolithic                        | Microservices                                |
| ---------------- | --------------------------------- | -------------------------------------------- |
| Architecture     | One large application             | Multiple smaller services                    |
| Deployment       | Usually one deployment unit       | Services can be deployed independently       |
| Codebase         | Usually one large codebase        | Separate service codebases/modules           |
| Scaling          | Often scale the whole application | Individual services can be scaled            |
| Communication    | Mostly internal calls             | APIs, HTTP/gRPC, messaging                   |
| Database         | Often shared                      | Services commonly own their data             |
| Development      | Simpler initially                 | More complex                                 |
| Infrastructure   | Simpler initially                 | More infrastructure required                 |
| Monitoring       | Relatively simple                 | More complex                                 |
| Failure handling | Simpler                           | Distributed failures need to be handled      |
| Best suited      | Smaller/simple systems            | Large systems requiring service independence |

---

# 4. Physical Machine vs Virtual Machine vs Container

## Physical Machine

A physical machine is an actual computer/server with:

* CPU
* RAM
* Storage
* Network
* Operating System

```text
Physical Server
│
├── CPU
├── RAM
├── Storage
├── Network
└── Operating System
```

---

## Virtual Machine

A Virtual Machine is created using a **hypervisor**.

Multiple VMs can run on the same physical server.

```text
                Physical Server
        ┌──────────────────────────┐
        │ Hardware                 │
        │                          │
        │      Hypervisor          │
        │      ┌─────┐             │
        │      │ VM1 │             │
        │      ├─────┤             │
        │      │ VM2 │             │
        │      ├─────┤             │
        │      │ VM3 │             │
        │      └─────┘             │
        └──────────────────────────┘
```

Each VM generally has its own complete guest operating system.

---

## Container

A container is an isolated process environment that packages an application with the dependencies it needs.

Containers share the **host operating system kernel** rather than each running a complete guest OS.

```text
                Host Operating System
        ┌──────────────────────────────┐
        │                              │
        │ Container 1  Container 2     │
        │     │             │          │
        │ Application    Application   │
        │ Dependencies   Dependencies  │
        │                              │
        └──────────────┬───────────────┘
                       ↓
                  Host Kernel
```

### Important Point

Containers are **not the same as VMs**.

A VM virtualizes hardware and normally contains a complete guest OS.

A container provides **OS-level process isolation** and shares the host kernel.

---

# 5. Why Containers?

Before containerization, applications could depend on many external components.

For example:

```text
Jenkins
  ↓
Requires Java

Tomcat
  ↓
Requires Java

AWS CLI
  ↓
Requires Python/runtime dependencies
```

If the required dependency is missing or incompatible, the application may fail.

Containerization allows the application and its required dependencies/configuration to be packaged into a consistent unit.

```text
Application
     +
Dependencies
     +
Required Files
     ↓
Container Image
```

### Simple Example

Instead of finding different ingredients from different places:

```text
Application
   ↓
Dependency 1
   ↓
Dependency 2
   ↓
Dependency 3
   ↓
Configuration
```

we package what the application needs into a reproducible image.

---

# 6. Containerization

**Containerization** is a method of packaging an application and its dependencies into an isolated, portable runtime environment.

It is often described as **OS-level virtualization** because containers use operating-system features such as namespaces and control groups for isolation and resource management.

---

# 7. What is Docker?

**Docker** is a platform and ecosystem used to build, package, distribute, and run applications as containers.

Docker is:

* Open source
* Cross-platform
* Widely used in Linux environments
* Available on Windows and macOS through Docker Desktop
* Commonly used in DevOps and CI/CD

Docker was introduced in **2013** and Docker's core components are primarily written in **Go**.

### Docker is used for:

* Building images
* Running containers
* Managing containers
* Creating networks
* Managing volumes
* Sharing images through registries
* Packaging applications consistently

---

# 8. Docker Alternatives

Other container technologies/tools include:

* Podman
* containerd
* CRI-O
* Buildah

Some are container runtimes while others provide container management/build capabilities, so they are not all direct one-to-one replacements for Docker.

---

# 9. Docker and YAML

Docker commonly uses **YAML** for Docker Compose configuration.

Example:

```text
docker-compose.yml
```

YAML can describe:

* Services
* Networks
* Volumes
* Environment variables
* Port mappings

---

# 10. Docker Uses Host Resources

Containers use resources provided by the host system:

```text
Host
│
├── CPU
├── Memory
├── Storage
└── Network
       ↓
   Containers
```

Docker does not give every container a separate physical CPU or RAM.

The operating system manages access to host resources.

---

# 11. Docker Architecture

```text
                  +----------------------+
                  |    Docker Client     |
                  |  (docker commands)   |
                  +----------+-----------+
                             |
                       Docker API
                             |
                             ↓
                  +----------------------+
                  |    Docker Daemon     |
                  |       dockerd        |
                  +----------+-----------+
                             |
        -----------------------------------------------
        |                  |                |          |
        ↓                  ↓                ↓          ↓
   Docker Images     Containers         Networks    Volumes
                             |
                             ↓
                      Container Runtime
                             |
                             ↓
                       Linux Kernel
                 Namespaces + cgroups
```

---

# 12. Docker Client

The **Docker Client** is the interface through which users interact with Docker.

Example:

```bash
docker run nginx
```

The client sends a request to the Docker daemon.

The client itself does not directly create the container.

---

# 13. Docker Daemon

The Docker daemon is the main Docker service.

It can manage:

* Images
* Containers
* Networks
* Volumes

It can perform operations such as:

* Pull images
* Build images
* Run containers
* Stop containers
* Remove containers
* Create networks
* Create volumes

Docker uses lower-level container runtime components such as **containerd** and **runc**.

---

# 14. Docker Host

The Docker Host is the machine where Docker is running.

It can be:

* Physical server
* Virtual machine
* Cloud instance
* Developer laptop

Example:

```text
AWS EC2
   ↓
Docker Engine
   ↓
Containers
```

---

# 15. Docker Registry

A Docker Registry stores and distributes container images.

Examples include:

* Docker Hub
* Amazon ECR
* GitHub Container Registry
* Azure Container Registry
* Google Artifact Registry

Example:

```text
Developer
    ↓
Docker Build
    ↓
Docker Image
    ↓
Docker Registry
    ↓
Pull Image
    ↓
Server
    ↓
Container
```

---

# 16. Docker Objects

Docker manages several important objects.

### Images

Templates used to create containers.

```text
nginx:latest
ubuntu:24.04
myapp:v1
```

### Containers

Running or stopped instances created from images.

### Networks

Allow containers and other endpoints to communicate.

### Volumes

Provide persistent storage for containerized applications.

---

# 17. Docker Image

A **Docker image** is a read-only, versioned package containing the filesystem and metadata required to create a container.

Example:

```text
nginx:latest
```

An image can contain:

* Application
* Runtime
* Libraries
* Configuration
* Files
* Metadata

An image is used as the template for creating containers.

---

# 18. Docker Container

A container is a running or stopped instance created from an image.

For example:

```text
Image
  ↓
docker run
  ↓
Container
```

One image can be used to create multiple containers.

```text
             nginx Image
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
    Container  Container  Container
       1           2          3
```

---

# 19. Docker Image vs Container

| Image                       | Container                      |
| --------------------------- | ------------------------------ |
| Template/package            | Running or stopped instance    |
| Read-only layers            | Has a writable container layer |
| Used to create containers   | Created from an image          |
| Can be stored in a registry | Runs on a Docker host          |
| Example: `nginx:latest`     | Example: `my-nginx-container`  |

---

# 20. Docker Workflow

When you execute:

```bash
docker run nginx
```

a simplified flow is:

```text
User
  ↓
Docker Client
  ↓
Docker Daemon
  ↓
Check Local Image
  ↓
Is Image Available?
       │
   ┌───┴───┐
   │       │
  YES      NO
   │       │
   │       ↓
   │   Pull Image
   │   from Registry
   │       │
   └───┬───┘
       ↓
Create Container
       ↓
Start Container
       ↓
Running Container
```

---

# 21. Linux Kernel and Containers

Containers use Linux kernel features to provide isolation and resource control.

Important technologies include:

### Namespaces

Namespaces provide isolation.

Examples:

* PID namespace
* Network namespace
* Mount namespace
* IPC namespace
* UTS namespace
* User namespace

They help separate what processes can see and access.

### cgroups

**Control groups (cgroups)** manage and limit resources.

Examples:

* CPU
* Memory
* Block I/O

This helps prevent one container from consuming uncontrolled amounts of host resources.

### OverlayFS

Overlay filesystems can provide layered filesystem functionality used by container storage systems.

---

# 22. Containers vs Virtual Machines

```text
                  VIRTUAL MACHINES

             Physical Server
                    │
                Hypervisor
          ┌─────────┼─────────┐
          ↓         ↓         ↓
         VM1       VM2       VM3
          │         │         │
       Guest OS   Guest OS   Guest OS
          │         │         │
       App       App        App
```

Containers:

```text
                    CONTAINERS

             Physical / VM Host
                    │
                Host OS
                    │
               Docker Engine
          ┌─────────┼─────────┐
          ↓         ↓         ↓
     Container   Container  Container
          │         │         │
        App       App        App
          └─────────┬─────────┘
                    ↓
                Host Kernel
```

### Main Difference

**VM:**

```text
Application
    ↓
Guest OS
    ↓
Virtual Hardware
    ↓
Hypervisor
```

**Container:**

```text
Application
    ↓
Container
    ↓
Host Kernel
```

Therefore containers are generally **lighter and faster to start than full VMs**, although actual resource usage depends on the workload and implementation.

---

# 23. Docker Editions

Historically Docker used terminology such as:

* Docker Community Edition (CE)
* Docker Enterprise Edition (EE)

Docker's product packaging has changed over time. For current Docker usage, you will commonly encounter **Docker Engine** and **Docker Desktop**, while commercial subscriptions provide additional features and support.

For learning Docker fundamentals, focus on:

* Docker Engine
* Docker CLI
* Dockerfile
* Docker Images
* Containers
* Docker Compose
* Networks
* Volumes
* Registries

---

# 24. Docker Container Lifecycle

A simplified lifecycle is:

```text
Docker Image
     ↓
docker create / docker run
     ↓
Container Created
     ↓
Container Running
     ↓
Container Paused
     ↓
Container Running
     ↓
Container Stopped
     ↓
Container Restarted
     ↓
Container Removed
```

Common commands:

```bash
docker create
docker run
docker start
docker stop
docker restart
docker pause
docker unpause
docker rm
```

---

# 25. OCI — Open Container Initiative

**OCI** stands for:

> **Open Container Initiative**

OCI is an open standards organization that defines specifications for container images, runtimes, and distribution.

It was established in **2015** under the Linux Foundation.

The goal is interoperability across container technologies.

---

# 26. Why OCI Was Needed

Without common standards, different container tools could potentially use incompatible:

* Image formats
* Runtime behavior
* Distribution mechanisms

OCI provides common specifications.

This makes it easier for container technologies to interoperate.

---

# 27. OCI Specifications

Important OCI specifications include:

### OCI Image Specification

Defines things such as:

* Image format
* Image configuration
* Image layers
* Image manifest

### OCI Runtime Specification

Defines how a container runtime should execute a container, including:

* Process execution
* Filesystem setup
* Runtime configuration

### OCI Distribution Specification

Defines how container artifacts can be distributed through registries.

---

# 28. Docker and OCI

Docker images and Docker's container ecosystem use standards that support OCI compatibility.

OCI-compliant or OCI-compatible technologies include tools/runtimes such as:

* containerd
* CRI-O
* Podman

The exact compatibility depends on the specific component and version.

---

# 29. Complete Docker Architecture

```text
                         USER
                           │
                           ↓
                    Docker Client
                           │
                           ↓
                      Docker API
                           │
                           ↓
                    Docker Daemon
                       dockerd
                           │
             ┌─────────────┼─────────────┐
             ↓             ↓             ↓
          Images       Containers     Networks
             │             │             │
             │             ↓             │
             │        Container Runtime │
             │             │             │
             └─────────────┼─────────────┘
                           ↓
                       Host Kernel
                    ┌──────┴──────┐
                    ↓             ↓
               Namespaces      cgroups
                    │             │
                    ↓             ↓
                 Isolation     Resources
```

---

# 30. Complete DevOps Flow

The concepts you learned connect together like this:

```text
Developer
    ↓
GitHub
    ↓
Jenkins
    ↓
Build Application
    ↓
Test
    ↓
SonarQube
    ↓
Docker Build
    ↓
Docker Image
    ↓
Docker Registry
    ↓
Docker / Kubernetes
    ↓
Production
```

For a microservices application:

```text
                   GitHub
                      │
                    Jenkins
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
    User Service  Order Service  Payment Service
        ↓             ↓             ↓
    Docker Image  Docker Image  Docker Image
        └─────────────┼─────────────┘
                      ↓
               Container Registry
                      ↓
                 Kubernetes
                      ↓
                  Production
```

---

# 31. Key Points to Remember

### Monolithic

> One large application containing multiple business functionalities and generally deployed as one unit.

### Microservices

> An architecture that divides an application into smaller services focused on individual business capabilities.

### Virtual Machine

> A virtualized computer that normally includes its own guest operating system.

### Container

> An isolated application process environment that shares the host kernel.

### Docker

> A platform and ecosystem for building, packaging, distributing, and running containerized applications.

### Docker Image

> A read-only, versioned package/template used to create containers.

### Docker Container

> An instance created from a container image.

### Docker Registry

> A system for storing and distributing container images.

### OCI

> Open standards that define how container images and runtimes should work.

---

# 32. Quick Revision

```text
MONOLITHIC
    ↓
One Large Application


MICROSERVICES
    ↓
Multiple Independent Services


VM
    ↓
Virtual Hardware + Guest OS + Application


CONTAINER
    ↓
Application + Dependencies
    ↓
Shares Host Kernel


DOCKER
    ↓
Build + Run + Manage Containers


IMAGE
    ↓
Template for Container


CONTAINER
    ↓
Running/Stopped Instance of Image


REGISTRY
    ↓
Stores and Distributes Images


NAMESPACES
    ↓
Isolation


CGROUPS
    ↓
Resource Control


OCI
    ↓
Container Standards
```

---
29. Can Docker containers run on Windows?
30. Is Docker the same as a virtual machine?
