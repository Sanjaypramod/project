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
                git branch: 'master', url: 'https://github.com/Sanjaypramod/project.git'
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
                    sh 'echo -n a9jXkxqxK8MWcCLC3xse3F | base64'
                    sh 'kubectl apply -f Wordpress'
                    sh 'kubectl apply -f DemoSite'
                    sh 'kubectl get node'
                    sh 'kubectl get pods -A'
                    sh 'pwd'
                   
                }
            }
        }   
    }
}
