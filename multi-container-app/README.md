# Visit Tracker Pro (Docker Multi-Container App)

## The Challenge Objective
Based on the original CoderCo Containers Challenge, the foundational scope of this project was to transition from monolithic applications to microservices by:

1. Dockerizing a Python Flask web application with a welcome route and a counter route.

2. Integrating a standalone Redis Database key-value store to track app traffic.

3. Orchestrating the services seamlessly using a single unified docker-compose.yml file.

---

## Core Engineering Technical Takeaways
Completing this module provided deep insight into container ecosystems and architectural design:


* **Microservices Separation of Concerns:** Learned how to detach the stateful database tier (Redis) from the stateless presentation tier (Flask/Nginx). This prevents the common anti-pattern of packing an entire stack into a single heavyweight image.


* **Volume Persistence & Data Survival:** Configured isolated Docker Volumes. This guarantees that if a container crashes, is explicitly terminated, or updates, the Redis transactional key-value visit logs persist seamlessly without data loss.


* **Infrastructure-as-Code Orchestration:** Built declarative docker-compose blueprints. This maps custom isolated bridge networks, exposes exact port configurations, and governs the specific operational startup order.


* **Nginx Reverse Proxy & Load Balancing:** Integrated an Nginx container acting as a reverse proxy to manage incoming traffic, distribute client requests efficiently across application instances, and shield the underlying application servers.

---

## Image Optimisation

I rebuilt the web image to make it smaller and more secure, then measured the result.

| Image | Disk usage | Content size (compressed) |
|---|---|---|
| Original (`python:3.8-slim`) | 212 MB | 52.8 MB |
| Optimised (multi-stage, distroless) | 106 MB | 25.8 MB |

Both versions behave identically. I checked this by running each on its own
and confirming the same result.

### Decisions

**1. Multi-stage build with a distroless runtime**
- Why: the final image contains only Python, my dependencies and `count.py`:
  no shell, no package manager, no pip. Fewer bytes to push and pull, and
  fewer tools for an attacker to use.
- Considered: `python:slim` (still ships a shell, apt and pip) and
  `python:alpine` (smaller, but uses a different C library that some
  compiled packages don't support).
- Trade-off: I can't open a shell inside the running container to debug it.

**2. Upgraded Python 3.8 → 3.13**
- Why: Python 3.8 stopped getting security fixes in October 2024, and pip was
 installing older Flask versions to stay compatible with it.
- Note: the build stage and runtime must use the same Python version

**3. Pinned dependencies in `requirements.txt`**
- Why: every build installs exactly the same versions, so the same code
  always produces the same image.
- It lists all 8 packages, not just Flask and redis, because Flask pulls in
  6 others.
- Trade-off: versions have to be updated deliberately.

**4. Runs as a non-root user**
- Why: if the app is ever compromised, the attacker doesn't have root inside
  the container.

### What I learnt
- Image layers only add. Deleting a file in a later step doesn't make the
  image smaller
- The base image was most of the size
- "Content size" is the compressed size that actually travels to a registry
  like ECR, so it's the number that affects push and pull times.
---

## Installation & Getting Started

### Prerequisites
Ensure you have the following frameworks natively installed on your machine:
* Docker Desktop (Includes the Compose CLI bundle)
* Git

### Step-by-Step Execution
1. Clone this repository locally to your machine:
   ```bash
   git clone https://github.com/Ameersworld/docker-learning/tree/main/multi-container-app
   ```
2. Navigate directly into the project directory:
   ```bash
   cd docker-learning/multi-container-app
   ```
3. Compile the Docker images and launch the cluster for the first time:
   ```bash
   docker-compose up --build
   ```
4. **Scale and Test Load Balancing:** To verify that Nginx is actively distributing client requests using round-robin routing across a pool of backend servers, spin up multiple concurrent instances of your web service:
   ```bash
   docker-compose up --scale web=3
   ```

Once the terminal logs confirm the services are healthy, open your preferred browser and navigate to:
**http://localhost:5002**

---

## Future Architecture Roadmap
To continue expanding my knowledge of enterprise DevOps methodologies, the upcoming milestones for this repository feature:

- [ ] **Secure Secret Management:** Extracting sensitive database parameters and server ports out of version control and loading them securely via runtime `.env` wrappers.

- [ ] **CI/CD Automation:** Setting up a GitHub Actions workflow to automatically test the code and lint the Dockerfiles on every push.
