pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'master', url: 'https://github.com/Sanjaypramod/kubeaudit.git'
            }
        }

        stage('Authenticate with EKS') {
            environment {
                AWS_DEFAULT_REGION = 'us-west-2'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    credentialsId: 'aws-creds',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh 'aws eks update-kubeconfig --name sanjay-cluster --region us-west-2'
                    sh 'kubectl get node'
                    sh 'kubectl get pods -A'
                    sh 'echo "Mypass123" | base64'
                    sh 'kubectl apply -f PersistentVolume.yaml'
                    sh 'kubectl apply -f mysql-service.yaml'
                    sh 'kubectl apply -f mysql.yaml'
                    sh 'kubectl apply -f namespace.yaml'
                    sh 'kubectl apply -f persistentVolumeClaim.yaml'
                    sh 'kubectl apply -f secret.yaml'
                    sh 'kubectl apply -f wordpress-deployment.yaml'
                    sh 'kubectl apply -f wordpress-service.yaml'
                   
                }
            }
        }   
    }
}
