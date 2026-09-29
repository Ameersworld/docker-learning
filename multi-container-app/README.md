# Visit Tracker Pro (Docker Multi-Container App)

### Repository Status Badges
The graphics below are dynamic status badges generated via Shields.io. In modern software engineering repositories, these are utilized to give hiring managers, contributors, and recruiters immediate, real-time metadata regarding the project's technology stack, build status, and code licensing without needing to dig into the source files.

![Docker](https://shields.io)
![Python](https://shields.io)
![Redis](https://shields.io)
![Nginx](https://shields.io)

A multi-container web application built to practice multi-container orchestration, microservices architecture, and state persistence using Docker, Docker Compose, Python Flask, Redis, and Nginx.

---

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
