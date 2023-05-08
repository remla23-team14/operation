# A1: Operation
This repository contains the docker compose file for the application.

## Usage
- Download the `docker-compose.yml` file.
- Login to the ghcr.io docker registry:
  ```sh
  docker login ghcr.io
  ```
- Run the following command in the same folder as the docker compose file:
  ```sh
  docker compose up
  ```
- The application will be available at [localhost](http:localhost:80).

## Useful files
### To understand the application, it may be useful to check the following repositories to see the backend code:
- [Model-training](https://github.com/remla23-team14/model-training)
  - [preprocessing.py](https://github.com/remla23-team14/model-training/blob/main/src/data_preprocessing.py): Preprocesses the input, and transforms it into a feature vector. 
  - [train.py](https://github.com/remla23-team14/model-training/blob/main/src/train.py): Trains the model based on the feature vector obtained from preprocessing.py.
- [Model-service](https://github.com/remla23-team14/model-service)
  - [app.py](https://github.com/remla23-team14/model-service/blob/main/app.py): Serves the model in a flask API.
  - [predictor.py](https://github.com/remla23-team14/model-service/blob/main/ml/api/predictor.py): Returns the result of a sentiment analysis.
- [App](https://github.com/remla23-team14/app)
