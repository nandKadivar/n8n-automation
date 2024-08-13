# n8n Automation: Pull Request Notification to Sling Channel

## Overview

This n8n automation workflow is designed to enhance the efficiency of our development process by automatically sending notifications to a specified Sling channel whenever a new pull request (PR) is created in our GitHub repository. The notification includes key details of the PR and indicates that it is ready for review and merging.

## Prerequisites

To use this n8n automation workflow, you need the following:

- **n8n instance**: Ensure that you have n8n set up and running on your local machine or a server.
- **GitHub Access Token**: Required for accessing your repository and monitoring pull requests.
- **Sling Webhook URL**: The URL to the Sling channel webhook where notifications will be sent.

## Setup Instructions

### 1. Clone the Repository

Clone this repository to your local machine:

```bash
https://github.com/nandKadivar/n8n-automation
```

## 2. Create docker volume
Creating a docker volume for our n8n local installation which makes it easy to deploy and migrate. For more information on n8n docker installation visit [n8n installation docs](https://docs.n8n.io/hosting/installation/docker/#prerequisites).

Run the following command to create a docker volume for your n8n instance:

```bash
docker volume create n8n_data
```

## 2. Expose n8n to the Internet with ngrok

To expose your local n8n instance to the internet, you can use ngrok:

### Install ngrok:

If you haven't installed ngrok, you can download it from the [ngrok website](https://ngrok.com/).

### Start ngrok:

Run the following command to create a tunnel for your n8n instance:

```bash
ngrok http 5678
```
Copy the forwarding URL generated by ngrok to map it with the n8n instance
Example URL: https://xxxx-xxx-xx-xx-xxx.ngrok-free.app

## 2. Run n8n instance with Docker

If you prefer to run n8n locally using Docker, follow these steps:

Update ngrok URL in the docker-compose file
### Docker compose file:

```bash
version: '1'
services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    environment:
      - WEBHOOK_URL=https://xxxx-xxx-xx-xx-xxx.ngrok-free.app/
    command: start
    restart: unless-stopped
volumes:
  n8n_data:
    external: true
```
Run the following command to start the n8n instance
### Pull the latest n8n Docker image:
```bash
docker-compose up
 ```
 
### Access the n8n UI:

Once the container is running, open your browser and navigate to [http://localhost:5678](http://localhost:5678) to access the n8n interface.

## 3. Configure the Workflow

- Open n8n and import the workflow JSON file provided in this repository.
- Update GitHub & Slack credentials with your personal access token.
- Replace the Sling channel name with the one for your target Sling channel.

## 4. Activate the Workflow

Once the workflow is configured, activate it in n8n. It will now monitor your GitHub repository for new pull requests.

## 5. Testing

Create a test pull request in your GitHub repository to ensure that the workflow triggers correctly and that the notification is sent to the Sling channel.

### New PR:

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/127b7ba9-57cf-40cb-a009-0025447d64a4">

### Workflow Execution:

<img width="1227" alt="image" src="https://github.com/user-attachments/assets/cf8404b7-c8d7-4e32-9277-c38156e7647a">

### Slack Channel Notification:

<img width="720" alt="image" src="https://github.com/user-attachments/assets/2f4a3f68-4055-40b7-99d9-e64b76303056">

### GitHub Webhook:

<img width="1439" alt="image" src="https://github.com/user-attachments/assets/0c064b12-bf0f-4a2f-b840-8f625045db53">

### Execution video

[Watch the demo video](https://www.youtube.com/watch?v=6P5Q-KnyzDw)

## Workflow Details

The workflow is structured as follows:

- **Trigger**: The workflow is triggered by creating a new pull request in the specified GitHub repository.
- **Extract Details**: It captures relevant information such as the PR title, status and URL.
- **Send Notification**: The workflow sends a message to the Sling channel using a webhook, containing the extracted details and a note that the PR is ready for review and merge.

## Customization

You can customize this workflow to fit your specific needs. For example, you can:

- Modify the message format sent to the Sling channel.
- Add additional conditions to trigger the notification.
- Integrate with other tools or services as needed.
