# Elastic Face Recognition Application

This project focuses on developing an elastic face recognition application using the IaaS resources from AWS. The application is designed to utilize a multi-tier architecture to dynamically scale based on demand and perform face recognition using a machine learning model.

## Project Structure

- **Web Tier:** Handles incoming image requests and sends them to the App Tier for processing. It also returns the recognition results to the users.
- **App Tier:** Processes image recognition using a pre-trained deep learning model.
- **Data Tier:** Manages data storage and retrieval from AWS S3 buckets.

## Project Architecture
<img width="709" alt="image" src="https://github.com/iisshh/Multi-Tier-Cloud-Based-Face-Recognition-System/assets/16882901/29b36847-6154-4066-a03b-6076ef4336ac">


## Technology Stack

- AWS EC2
- AWS S3
- AWS SQS
- Python
- Deep Learning Models (PyTorch)

## Setup Instructions

### Prerequisites

- AWS Account
- AWS CLI configured
- Python 3.x
- Access to AWS services (EC2, S3, SQS)

### Deployment

#### Configure AWS Credentials

### Launch the EC2 Instance

Ensure the EC2 instance is running and has the appropriate IAM roles attached for accessing S3 and SQS.

### Setup S3 Buckets

Create two S3 buckets for input and output as described in the project guidelines.

### Setup SQS Queues

Create request and response queues in SQS to handle communication between web and app tiers.

### Deploy the Application

1. Adjust the security group settings to allow traffic on port 8000.
2. Deploy the Python Flask app on the EC2 instance to handle HTTP requests.

### Usage

Send a POST request to the deployed application with an image file. The application will return the face recognition result in plain text format.

#### Example POST request using curl

curl -X POST -F "inputFile=@path_to_image.jpg" http://ec2-instance-public-ip:8000/

## Additional Notes

### Edit `nginx.conf`

To edit the `nginx.conf` file, use the following command:

sudo nano /etc/nginx/nginx.conf

Install Gunicorn
Make sure to install Gunicorn:
pip install gunicorn
gunicorn -b 0.0.0.0:8000 app:app
Service Files
Navigate to /etc/systemd/system and add the following service files:

ishproj1.service
Runs project1phase2.py:

Logs: myWebTier.log
Functionality:
Receives POST requests from the LB
Uploads the images to S3
Sends the images to the request SQS
Polls the DB to check if the request is served; if yes, fetches the result and deletes it from the DB
webTierCron.service
Runs webTierCron.py:

Functionality: Responsible for autoscaling of instances
consumer.service
Runs /home/ubuntu/ishproj1/consumeOutputQueue.py:

Logs: consumeOutputQueue.log
Functionality:
Retrieves messages from the output SQS queue and stores them in the DB
SQLite DB
Database: ImageClassification.db
Manage Services
To check the status of any service:
sudo systemctl status myScript.service
Whenever you edit the nginx file, run the following commands:
sudo systemctl daemon-reload
sudo systemctl enable xxx.service
sudo systemctl start xxx.service
sudo systemctl status xxx.service


