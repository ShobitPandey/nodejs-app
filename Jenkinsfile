pipeline {
    agent any

   stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository containing your app
                git url: 'https://github.com/ShobitPandey/nodejs-app.git', branch: 'main'
            }
        }


        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nodejs-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u <your-dockerhub-username> --password-stdin
                        docker tag nodejs-app:latest shobitpandey18/nodejs-app:latest
                        docker push shobitpandey18/nodejs-app:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
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
