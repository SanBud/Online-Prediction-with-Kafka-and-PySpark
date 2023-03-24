# Parking Violation Predictor with Kafka streaming and spark

# Architecture

The data for NY Parking violation is very huge. To use we
have to configure the spark cluster and distribute the data. For this
assignment, we have used only one cluster to train the data and predict using
pretrained model.

Following design approach is used to solve the problem,

1.       Ingest the data to spark data frame.

2.       Data preparation is done after EDA and selected
the required features from available 51 features.

3.       There are two model created at training process.

a.       Data processing model transformer.

This is the transformer created after creating pipelining for converting
categorical and numerical to vector assembler.  This model will be used to transform the predicting raw features.

Eg. Model = Training.fit(data)

      Raw_to_vectorIndexed =
Model.transform(raw features)

b.       ML model for predicting.

Use this model to predict on Raw_to_vectorIndexed.

4.       Containerization for data streaming

a.       Docker container is created using docker
composer to include following service,

                                                               i.      Kafka

                                                             ii.      Zookeeper

                                                           iii.      Spark with Jupiter. 

b.       When docker images are run, the kafka will read
the data from csv and stream.

c.       Spark streamer is configured as kafka consumer
to save the data and predict using pre trained model.

# Folder content

.

├── docker                                        # Contains docker composer

├── docs                                           # PDF
capture of training and predicting notebooks.

├── notebooks                                # All notebooks required to run along with
sample data

├── pretrained_models                # contains saved models to load
and predict

# How to run

1.       Unzip the content and go to docker folder. (cd
spark_assignement/docker)

2.       Modify the composer file with your ip under
kafka service as shown below

KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://YOUR-IP:9092  

3.       Run “docker-compose up”. Run this one shell keep
checking the logs to make sure everything is up and running.

It will start the following services:

**zookeeper**:

Image: wurstmeister/zookeeper:latest

Port: 2181

**kafka**:

Image: wurstmeister/kafka:2.13-2.8.1

Port: 9092

**spark**:

Image: jupyter/all-spark-notebook:spark-3.1.1

Port: 8888

4.       Open other terminal and navigate to root
directory of spark_assignment.

To run the notebooks for streaming and
consuming, jupyter need to started.

Docker exec -it docker_spark_1 bash

In the docker image run below command.

Jupyter notebook list

Above command will give url and paste it on
browser and replace 0.0.0.0 initial ip with localhost or your public ip.

5.       Run kafka_producer.ipynb to start producing.

6.       Run kafka_spark_consumer.ipynb to start
consuming and predicting the value.


