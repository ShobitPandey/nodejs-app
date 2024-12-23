pipeline {
    agent any

    environment {
        // Ensure npm, Docker, and kubectl are accessible in the PATH
        PATH = "/usr/local/bin:$PATH"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository containing your app
                git url: 'https://github.com/ShobitPandey/nodejs-app.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                sh 'docker build -t shobitpandey18/nodejs-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Log in to Docker Hub and push the image
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh '''
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            docker push shobitpandey18/nodejs-app:latest
                        '''
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Deploy the app to Kubernetes
                sh '''
                    kubectl apply -f k8s/
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
            mail to: 'pandeyshobit306@gmail.com',
                 subject: "Deployment Success",
                 body: "The application has been successfully deployed."
        }
        failure {
            echo "Deployment failed!"
            mail to: 'pandeyshobit306@gmail.com',
                 subject: "Deployment Failure",
                 body: "The deployment failed. Please check the logs for more details."
        }
    }
}
