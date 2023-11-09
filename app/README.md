# App Microservice 

This is a Golang app that uses Vue.js in the frontend and MySQL as the database.

It currently contains both the backend and the frontend in the same repository, but the idea is to decouple them in the future.

## Understanding the code

The `webserver.go` file is the entrypoint for the backend and it handles incoming requests, serving the static files for the Vue.js frontend at [src](./src) and invoking the [packages](./webserver) at the [webserver](./webserver) folder. It contain the logic that handle the requests, connecting to the database and returning the data.

## Building the app

This project tests the application using Docker and Skaffold. The advantage is that you don't need to install Go or MySQL in your machine.

Furthermore, if you run with Skaffold, it will create a local CICD pipeline that will automatically rebuild and redeploy the app everytime you change the code.

To build the app you need to run the following command:

```bash
go build -o app
```

## Running the app

To run the app you need to run the following command:

```bash
./app
```

To run with Docker or Skaffold, read the [Getting Started](#getting-started) section.