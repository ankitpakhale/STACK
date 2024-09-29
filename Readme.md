# Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/ankitpakhale/aws-stock-market-kafka-architecture.git
   ```
2. Navigate to the project directory:
   ```bash
   cd aws-stock-market-kafka-architecture
   ```
3. Create a virtual environment:
	```bash
	python -m venv .venv
	```
4. Activate the virtual environment:
   - On Windows:
		```bash
		.venv\Scripts\activate
		```
   - On macOS/Linux:
		```bash
		. .venv/bin/activate
		```
5. Install the required dependencies:
	```bash
	pip install -r requirements.txt
	```
	
# Project Flow
- **Dataset (CSV file)**: A stock market dataset is used as the input.
  
- **Stock Market App Simulation (Python)**: The stock market simulation app in Python reads the dataset and acts as the producer.
  
- **SDK (Boto3)**: The Python script uses the AWS Boto3 SDK to interact with AWS services.

- **Kafka (Producer/Consumer)**: 
  - The stock market app acts as the producer, sending messages to Kafka hosted on an Amazon EC2 instance/Local Machine.
  - Kafka then sends the data to consumers for processing.

- **Amazon S3**: The Kafka consumer writes the data to an Amazon S3 bucket.

- **AWS Glue Crawler**: A crawler in AWS Glue scans the data in S3, extracting metadata and building a data catalog.

- **AWS Glue Data Catalog**: This metadata is stored in the AWS Glue Data Catalog, providing a structured way to access the data.

- **Amazon Athena**: Amazon Athena is used to run SQL queries against the data stored in the S3 bucket, leveraging the Glue Data Catalog for schema information.

	
# To run project on EC2
- Create new t2.micro machine
- Download .pem file
- Run this command: chmod 400 kafka-stock-market-project.pem


## Download Kafka:
- Open new terminal
- Connect to EC2 using .pem file 
-
	```bash
	wget https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.6.2.tgz
	```
- 
	```bash
	tar -xvf kafka_2.13-3.6.2.tgz
	```

## Start the Zookeeper:
- Open new terminal
- Connect to EC2 using .pem file 
- 
	```bash
	cd kafka_2.13-3.6.2
	```
- 
	```bash 
	bin/zookeeper-server-start.sh config/zookeeper.properties
	```

## Start the kafka Server:
- Open new terminal
- Connect to EC2 using .pem file
- 
	```bash
	cd kafka_2.13-3.6.2
	```
- 
	```bash
	bin/kafka-server-start.sh config/server.properties
	```

## Create the topic:
- Open new terminal
- Connect to EC2 using .pem file 
- 
	```bash
	cd kafka_2.13-3.6.2
	```
- 
	```bash
	bin/kafka-topics.sh --create --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092} --replication-factor 1 --partitions 1
	```
- **Note:** Do not forget to change the topic name, currently it is  **demo_testing2**

## Start Producer & Consumer:
- Rename .env_example to .env and update the bucket name 
- Open new terminal
- jupyter notebook
- Change localhost to public IP of EC2 Instance in Create Kafka Producer section in KafkaProducer.ipynb file and run it. 
- Change localhost to public IP of EC2 Instance in Create Kafka Consumer section in KafkaConsumer.ipynb file and run it. 



# To run project on local machine

## Start the Zookeeper:
- Open new terminal
- 
	```bash
	cd kafka_2.13-3.6.2
	```
- 
	```bash
	bin/zookeeper-server-start.sh config/zookeeper.properties
	```

## Start the Kafka Server:
- Open new terminal
- 
	```bash
	cd kafka_2.13-3.6.2
	```
- 
	```bash
	bin/kafka-server-start.sh config/server.properties
	```

## Start Producer & Consumer:
- Rename .env_example to .env and update the bucket name
- Open new terminal
- jupyter notebook
- run KafkaProducer.ipynb
- run KafkaConsumer.ipynb


# Create Crawler
- Create Crawler and assign permissions to access S3 bucket.
- Create database to store data.
- Run the Crawler.

# Athena
- Go to Athena Console.
- Select Database name.
- Run the Athena Query.

# Contributing
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Commit your changes (`git commit -am 'Add new feature'`).
4. Push to the branch (`git push origin feature-branch`).
5. Create a new Pull Request.

# License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.