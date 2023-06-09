# Operation
This repository contains the deployment files for the application.
The application is a simple web application that allows users to view restaurants and add reviews.

## Prerequisites
- [Docker](https://www.docker.com)
- Docker compose / [Istio](https://istio.io) + [Helm](https://helm.sh)
- A valid session to the ghcr.io docker registry:
  ```sh
  docker login ghcr.io
  ```

## Usage (Istio + Helm)
### Istio Installation
To use the application, you need to have Istio installed.
We recommend installing the `istioctl` client binary using their [documentation](https://istio.io/latest/docs/setup/getting-started/#download).

Once installed, enable the demo profile:
```sh
istioctl install --set profile=demo -y
```
If you prefer to use another installation, make sure the namespace of istio is `istio-system`.

Then, enable automatic sidecar injection for the default namespace:
```sh
kubectl label namespace default istio-injection=enabled
```

### App Installation
1. Add the repository to helm:
   ```sh
   helm repo add remla23-team14 https://remla23-team14.github.io/operation
   ```
2. Run helm update:
   ```sh
   helm repo update
   ```
3. Install the application:
   ```sh
   helm install app remla23-team14/app
   ```
   Please refer to the values.yaml file for the available configuration options.
   You can set these by duplicating the [charts/app/values.yaml](../charts/app/values.yaml) file and changing the values.
   Then, you can install the application with the following command:
   ```sh
   helm install app remla23-team14/app -f values.yaml
   ```

### Access the application
To access the application, connect to the ingress on port 80.
If you are using [minikube](https://github.com/kubernetes/minikube),
you can use the following command:
```sh
minikube tunnel
 ```
This will expose the ingress on [http://localhost:80](http:localhost:80).

### Access Prometheus, Grafana and Model-service
To access Prometheus, Grafana and model-service by default, the domains `prometheus.local` and `grafana.local` are used.
Make sure you have the following lines in your `/etc/hosts` file, or the equivalent for your OS:
```sh
127.0.0.1 prometheus.local
127.0.0.1 grafana.local
127.0.0.1 model-service.local
```
You can then access Prometheus on [http://prometheus.local](http://prometheus.local),
Grafana on [http://grafana.local](http://grafana.local) and 
the Model-service docs on [http://prometheus.local/apidocs](http://model-service.local).

## Usage (docker compose)
### Installation
Run the following command in the repository root:
```sh
docker compose up
```

If you want to specify a different model for the model-service,
you can uncomment the following lines in the `docker-compose.yml` file:
```yml
services:
  model-service:
    ...
#    volumes:
#      - ./model-training/c1_BoW_Sentiment_Model.pkl:/root/model-training/c1_BoW_Sentiment_Model.pkl
#      - ./model-training/c2_Classifier_Sentiment_Model:/root/model-training/c2_Classifier_Sentiment_Model
#      - ./model-training/pre_processed_dataset.joblib:/root/model-training/pre_processed_dataset.joblib
#      - ./model-training/model_metrics.json:/root/model-training/model_metrics.json
```
Make sure you have the model files in the `model-training` folder (or change it).
**Note: the target names must stay the same!**
See [this other README](https://github.com/remla23-team14/model-service/blob/main/model-training/README.md), 
on how you can download models using DVC.

Next to mounting your own files you can also automatically download different DVC models. 
You can do this by uncommenting and changing the following lines in the `docker-compose.yml` file:

```yml
    environment:
      MODEL_SERVICE_URL: http://model-service:8080
#      DVC_REPO: "https://github.com/remla23-team14/model-training.git"
#      DVC_REV: "HEAD"
#      DVC_MODEL_PATH: "models/c2_Classifier_Sentiment_Model"
#      DVC_PREPROCESSOR_PATH: "data/processed/c1_BoW_Sentiment_Model.joblib"
#      DVC_CORPUS_PATH: "data/processed/pre_processed_dataset.joblib"
#      DVC_METRICS_PATH: "data/output/model_metrics.json"
#      USE_DVC: "false"
```

These variables do the following:
* `MODEL_SERVICE_URL` - The url and port on which the model service run.
* `DVC_REPO` - The DVC repo the files should be pulled from (use the http version).
* `DVC_REV` - When using a git repo. This refers to the `Git revision` from where it should be pulled.
* `DVC_MODEL_PATH` - The path to the model file in the DVC repo.
* `DVC_PREPROCESSOR_PATH` - The path to the preprocessor file in the DVC repo.
* `DVC_CORPUS_PATH` - The path to the corpus file in the DVC repo.
* `DVC_METRICS_PATH` - The path to the metrics file in the DVC repo.
* `USE_DVC` - Whether to use DVC. If false it will try to use local files if they can be found, 
  otherwise it will still result to dvc


### Access
The application will be available at [http://localhost:80](http:localhost:80).

## Organization structure
To understand the application, it may be useful to check the following repositories their README files:
- [model-training](https://github.com/remla23-team14/model-training): contains the code for training the model.
- [model-service](https://github.com/remla23-team14/model-service): serves the sentiment model.
- [lib](https://github.com/remla23-team14/lib): simple version library for the app.
- [app](https://github.com/remla23-team14/app): contains the frontend and backend code for the application, which queries the model-service backend for a sentiment analysis.

## Operaton Setup
Grafana dashboards can be added as json in the `charts/app/dashboards` folder.
They will automatically be loaded into Grafana.

To deploy a new version of the Helm chart,
simply bump the version in Chart.yaml and the new version will be deployed.

## Screenshots
### Landing page
![landing page](images/landing.png)

### Restaurants Overview
![restaurants overview](images/restaurants.png)

### Restaurant Reviews
![Restaurant reviews](images/reviews.png)
