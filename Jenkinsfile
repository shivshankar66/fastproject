pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-creds')  // Jenkins me add kiya hua username/password
        IMAGE_NAME = "fastproject"
        IMAGE_TAG = "30"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/shivshankar66/fastproject.git'
            }
        }

        stage('Docker Build & Push') {
            steps {
                bat "docker build -t $DOCKERHUB_CREDS_USR/$IMAGE_NAME:$IMAGE_TAG ."
                bat "echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin"
                bat "docker push $DOCKERHUB_CREDS_USR/$IMAGE_NAME:$IMAGE_TAG"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f deploy_ks.yml"
                bat "kubectl get pods"
                bat "kubectl get svc"
            }
        }
    }

    post {
        success { echo "✅ Pipeline executed successfully!" }
        failure { echo "❌ Pipeline failed!" }
    }
}
