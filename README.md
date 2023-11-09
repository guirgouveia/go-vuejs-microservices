# Go Blog with Vue.js

This project creates a personal blog with Golang microservices in the backend and Vue.js in the frontend connecting to a MySQL database.

## Roadmap

The project is still in the early stages of development, so there is a lot of work to do, for example, the frontend is still not decoupled from the backend.

Take a look at the [Roadmap](./docs/roadmap.md) to see the features that are being developed.

The services are be designed to reside in the cloud from the start, so Kubernetes Manifets and Helm Charts are being developed along with the code.

## Services

Read the documentation for each service at:

- [app](./app/README.md)

## Getting Started

You can run this project locally using `Docker` or `Kubernetes` or you can deploy it to the cloud using `Kubernetes`.

### Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

#### Optional

If you want to run the project using `Kubernetes` you need to install the following tools:

- `Kubernetes Cluster`
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Skaffold](https://skaffold.dev/docs/install/)
- [Helm](https://helm.sh/docs/intro/install/)

### Running with Docker Compose

To run the project locally using `Docker Compose` you need to run the following command:

```bash
docker-compose up --build -d
```

Access the frontend at http://localhost:8081.

### Running with Skaffold

To run the project locally using `Skaffold` you need to run the following command:

```bash
skaffold dev
```

Access the frontend at http://localhost:8083.
