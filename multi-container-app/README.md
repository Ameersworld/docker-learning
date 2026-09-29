# Visit Counter

Visit counter is a simple containerised web app, by default it directs you to a welcome page whereas /count directs you to a page where the counter updates dynamically depending on the amount of times the page has been visited.

# Multi-containers
A multi-container approach via docker-compose file has been used to allow the visit count to persist between instances a docker volume with a redis database has been used.

## Installation

1. Ensure docker desktop is installed
2. Pull the repo
3. Direct yourself to the multi-container-app then run:

```bash
docker-compose up --build
```

## Usage
Once the app is up and running, visit it at localhost:5002 in your web-browser