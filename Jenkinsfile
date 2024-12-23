pipeline {
    agent any

    environment {
    
        PATH = "/usr/local/bin:$PATH"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository containing your app
                echo "Cloning the repository..."
                git url: 'https://github.com/ShobitPandey/nodejs-app.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests
                echo "Running tests..."
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                echo "Building Docker image..."
                sh 'docker build -t shobitpandey18/nodejs-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Log in to Docker Hub and push the image
                echo "Pushing Docker image to Docker Hub..."
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
                echo "Deploying the app to Kubernetes..."
                sh '''
                    kubectl apply -f k8s/
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
            script {
                def accountSid = 'AC8e9fa6ba00ed6e1991e1fc454f217008'
                def authToken = 'ec4abce0395c49b71890aa577498fe7d'
                def fromPhone = '+12184964707'
                def toPhone = '+919634545641'

                sh """
                    curl -X POST https://api.twilio.com/2010-04-01/Accounts/${accountSid}/Messages.json \
                    --data-urlencode 'To=${toPhone}' \
                    --data-urlencode 'From=${fromPhone}' \
                    --data-urlencode 'Body=Deployment Successful!' \
                    -u ${accountSid}:${authToken}
                """
            }
        }
        failure {
            echo "Deployment failed!"
            script {
                def accountSid = 'AC8e9fa6ba00ed6e1991e1fc454f217008'
                def authToken = 'ec4abce0395c49b71890aa577498fe7d'
                def fromPhone = '+12184964707'
                def toPhone = '+919634545641'

                sh """
                    curl -X POST https://api.twilio.com/2010-04-01/Accounts/${accountSid}/Messages.json \
                    --data-urlencode 'To=${toPhone}' \
                    --data-urlencode 'From=${fromPhone}' \
                    --data-urlencode 'Body=Deployment Failed!' \
                    -u ${accountSid}:${authToken}
                """
            }
        }
    }
}
