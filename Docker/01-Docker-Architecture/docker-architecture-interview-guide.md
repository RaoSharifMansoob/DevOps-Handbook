# Docker Architecture - Interview Guide

These are some of the most frequently asked Docker Architecture interview questions for DevOps, Cloud, and Platform Engineering roles.

---

## 1. Explain the execution flow of `docker run nginx`.

**Answer:**

When `docker run nginx` is executed, the Docker CLI sends the request to the Docker Daemon through the Docker REST API. The Docker Daemon checks whether the image exists locally; if not, it pulls it from Docker Hub. It prepares networking, storage, and metadata, then delegates execution to **containerd**. Containerd starts **containerd-shim**, which invokes **runc**. Finally, `runc` uses Linux kernel features like namespaces and cgroups to create and start the container process.

---

## 2. What is the responsibility of Docker Daemon (`dockerd`)?

**Answer:**

Docker Daemon is the central management component of Docker. It builds images, pulls and pushes images, manages containers, creates Docker networks and volumes, processes Dockerfiles, maintains image cache, and delegates container execution to **containerd**. It does not directly create Linux processes.

---

## 3. What is the difference between Docker Daemon and containerd?

**Answer:**

Docker Daemon manages the overall Docker platform, including images, networks, volumes, and registries. Containerd is a dedicated container runtime responsible for managing the lifecycle of containers, such as creating, starting, stopping, and monitoring them.

---

## 4. Why is `containerd-shim` required?

**Answer:**

`containerd-shim` keeps the container running independently of containerd. It acts as the parent process of the container, manages standard input/output streams, and reports the container's exit status back to containerd.

---

## 5. What is the role of `runc`?

**Answer:**

`runc` is the OCI-compliant runtime that creates the actual container process. It configures namespaces, applies cgroups, mounts the filesystem, and starts the application. Once the container starts successfully, `runc` exits.

---

## 6. What are Linux namespaces?

**Answer:**

Namespaces provide isolation between containers. They isolate resources such as processes, network interfaces, hostnames, mount points, users, and IPC, making each container appear as if it has its own independent environment.

---

## 7. What are cgroups?

**Answer:**

Control Groups (cgroups) limit and monitor the resources a container can use. They control CPU, memory, disk I/O, and other system resources, preventing one container from consuming excessive resources.

---

## 8. What is OCI and why is it important?

**Answer:**

OCI (Open Container Initiative) defines open standards for container images and runtimes. Because Docker, Podman, containerd, and many other tools follow OCI standards, they can use the same container images and runtimes, ensuring portability and compatibility.

---

## 9. Why are Docker images built in layers?

**Answer:**

Docker images use layered filesystems to improve efficiency. Each Dockerfile instruction creates a new layer, allowing Docker to cache unchanged layers during rebuilds. This reduces build time, saves bandwidth, and minimizes storage usage.

---

## 10. Which Docker component is responsible for networks, volumes, and images?

**Answer:**

The **Docker Daemon (`dockerd`)** manages Docker resources such as images, containers, networks, and volumes. It is responsible for creating and maintaining these resources before delegating container execution to **containerd**.

---