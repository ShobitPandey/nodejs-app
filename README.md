# Simple CI/CD Pipeline for Node.js App

This repository demonstrates a **Simple CI/CD Pipeline** for a Node.js application. The pipeline automates building, testing, and deploying your app.

## Features

1. Clone Repository: Fetches the latest code from Git.
2. Install Dependencies: Installs required Node.js dependencies (`npm install`).
3. Run Tests: Runs tests to ensure functionality (`npm test`).
4. Build Docker Image: Creates a Docker image using the Dockerfile.
   - the pipeline uses a Dockerfile to create a Docker image.
   -  the Dockerfile contains instructions how to build this image.
5. Push Docker Image: Pushes the image to Docker Hub.
   - After the Docker image is built, it is pushed to Docker Hub
   - To push the image, you'd run a command like this:
      docker tag my-app:latest myusername/my-app:latest
      docker push myusername/my-app:latest
6. Deploy to Kubernetes: Deploys the app to a Kubernetes cluster.
   -  Deploy the app using Kubernetes configuration files
7. Notifications: Sends SMS alerts (success/failure) via Twilio.
   - the pipeline is to send  SMS notifications  through Twilio on your Mobile number.
   - Twilio sends an SMS alerting you to the Successful/failure.
