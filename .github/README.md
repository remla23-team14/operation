# Operation
This repository contains the deployment files for the application.
The application is a simple web application that allows users to view restaurants and add reviews.

## Prerequisites
- [Docker](https://www.docker.com)
- Docker compose / [Helm](https://helm.sh)
- A valid session to the ghcr.io docker registry:
  ```sh
  docker login ghcr.io
  ```

## Usage (Helm)
- Deploy the application's chart using the following command in the repository root:
  ```sh
  helm install app .
  ```
- Connect to the app-ingress on port 80. If you are using [minikube](https://github.com/kubernetes/minikube), you can use the following command:
  ```sh
  minikube tunnel
  ```
- To stop the application, run the following command:
  ```sh
  helm uninstall app
  ```

## Usage (docker compose)
- Run the following command in the repository root:
  ```sh
  docker compose up
  ```
- The application will be available at [localhost:80](http:localhost:80).
- To stop the application, run the following command:
  ```sh
  docker compose down
  ```

## Organization structure
To understand the application, it may be useful to check the following repositories their README files:
- [model-training](https://github.com/remla23-team14/model-training): contains the code for training the model.
- [model-service](https://github.com/remla23-team14/model-service): serves the sentiment model.
- [lib](https://github.com/remla23-team14/lib): simple version library for the app.
- [app](https://github.com/remla23-team14/app): contains the frontend and backend code for the application, which queries the model-service backend for a sentiment analysis.

## Screenshots
### Landing page
![landing page](images/landing.png)

### Restaurants Overview
![restaurants overview](images/restaurants.png)

### Restaurant Reviews
![Restaurant reviews](images/reviews.png)