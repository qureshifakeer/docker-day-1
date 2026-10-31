

```markdown
# Docker Fundamentals & Machine Test Lab

A hands-on implementation and verification of Docker architecture, container lifecycle management, and web server deployment.

---

## 1. Concepts Learned

* **Containerization vs Environment Drift:**  
  Solves the "It works on my machine" problem by bundling the application code along with its runtime, system tools, libraries, and configurations into a consistent, reproducible package that runs identically across development, testing, and production environments.

* **Virtual Machines vs Containers:**  
  * **Virtual Machines (VM):** Rely on a hypervisor to virtualize hardware and run complete Guest Operating Systems, resulting in high resource overhead and slower startup times.  
  * **Containers:** Share the host OS kernel and isolate processes in user space, offering a lightweight footprint, fast startup times, and efficient resource utilization.

* **Docker Core Architecture:**  
  * **Dockerfile:** A text file containing sequential instructions to build a Docker image.  
  * **Docker Image:** An immutable, versioned, read-only template used to instantiate containers.  
  * **Docker Container:** A runnable, isolated instance of an image with a thin writable layer.  
  * **Docker Registry:** A centralized storage system (e.g., Docker Hub) for distributing and versioning container images.  
  * **Docker Daemon (`dockerd`):** The background service responsible for building images, managing networks, handling storage, and executing containers.  
  * **Docker CLI:** The client-side command-line tool that communicates with the daemon via REST API calls over a UNIX socket.

---

## 2. System Readiness Check

Pre-installation verification conducted on a Windows 11 host with WSL 2 backend:

* **Windows Environment:**  
  * Verified WSL 2 status using `wsl --status` and `wsl --version`.  
  * Confirmed CPU hardware virtualization support through `systeminfo` (`Hypervisor detected`).
* **Linux Environment (WSL 2):**  
  * Kernel check via `uname -r` (`6.18.33.2-microsoft-standard-WSL2`).  
  * Available memory check via `free -h` (~3.1 GiB available).  
  * Disk space check via `df -h` (~953 GiB available on the root mount).

---

## 3. Practical Lab Tasks Executed

1. **Environment Verification:**  
   Checked client and server versions using `docker version` and system configurations with `docker info`. Verified container execution with `docker run hello-world`.

2. **Image Management:**  
   Pulled `nginx:alpine` from Docker Hub, inspected local images using `docker images`, and created an alias tag named `devops-nginx:day2` using `docker tag`.

3. **Web Server Deployment:**  
   Deployed an Nginx container running in detached mode mapped to host port `8082`:
   ```bash
   docker run -d --name opsdesk-nginx -p 8082:80 nginx:alpine

```

Verified successful deployment by accessing `http://localhost:8082` in the browser.

4. **Container Lifecycle & Inspection:**
* Viewed access logs using `docker logs opsdesk-nginx`.
* Executed a command inside the container using `docker exec opsdesk-nginx nginx -v`.
* Extracted detailed container state via `docker inspect`.
* Demonstrated the lifecycle difference between stopping a container (`docker stop`) and removing it (`docker rm`).



---

## 4. Command Execution Log

| Command | Purpose | Observed Result |
| --- | --- | --- |
| `docker --version` | Display installed Docker CLI version | Displayed client version |
| `docker version` | Show client and engine versions | Displayed detailed client and server daemon info |
| `docker info` | Inspect system-wide Docker configuration | Rendered system resources, containers, and driver details |
| `docker run hello-world` | Test engine functionality and image execution | Pulled image and printed confirmation output |
| `docker ps -a` | List all containers (active and inactive) | Listed containers including exited instances |
| `docker pull nginx:alpine` | Download Nginx image from registry | Downloaded official alpine-based image layers |
| `docker images` | List locally stored Docker images | Displayed repository, tag, image ID, and size |
| `docker tag nginx:alpine devops-nginx:day2` | Assign an alias tag to a local image | Created new tag referencing the same Image ID |
| `docker run -d --name opsdesk-nginx -p 8082:80 nginx:alpine` | Run containerized Nginx on host port 8082 | Container started in background; returned container ID |
| `docker ps` | List currently running containers | Showed `opsdesk-nginx` running on port mapping `0.0.0.0:8082->80/tcp` |
| `docker logs opsdesk-nginx` | View container output logs | Displayed web server request and initialization logs |
| `docker exec opsdesk-nginx nginx -v` | Execute command inside running container | Outputted Nginx binary version |
| `docker inspect opsdesk-nginx` | Inspect container low-level details | Displayed full configuration and state JSON |
| `docker stop opsdesk-nginx` | Stop container execution gracefully | Container process stopped; status updated to `Exited` |
| `docker start opsdesk-nginx` | Restart an existing stopped container | Container resumed execution without recreating state |
| `docker rm opsdesk-nginx` | Permanently remove the stopped container | Container removed from system listing |

```

```
