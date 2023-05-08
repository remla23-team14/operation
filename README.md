## Usage
- Download the docker-compose.yml
- Run "docker compose up" in the folder of the docker-compose file.
## Useful files
### To understand the application, it may be useful to check the following repositories to see the backend code:
- [Model-training](https://github.com/remla23-team14/model-training)
  - [preprocessing.py](https://github.com/remla23-team14/model-training/blob/main/src/data_preprocessing.py): Preprocesses the input, and transforms it into a feature vector. 
  - [train.py](https://github.com/remla23-team14/model-training/blob/main/src/train.py): Trains the model based on the feature vector obtained from preprocessing.py.
- [Model-service](https://github.com/remla23-team14/model-service)
  - [app.py](https://github.com/remla23-team14/model-service/blob/main/app.py) : Serves the model in a flask API.
  - [predictor.py](https://github.com/remla23-team14/model-service/blob/main/ml/api/predictor.py): Returns the result of a sentiment analysis.
- [App](https://github.com/remla23-team14/app)
