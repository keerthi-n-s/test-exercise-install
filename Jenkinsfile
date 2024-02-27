pipeline {
    agent any

    environment {
        KUBECONFIG = "${WORKSPACE}/kubeconfig.yaml"
        NAMESPACE = "monitoring"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your Jenkinsfile along with necessary files (e.g., kubeconfig.yaml)
                checkout scm
            }
        }

        stage('Set Up Environment') {
            steps {
                // Download kubectl
                sh 'curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl'
                sh 'chmod +x ./kubectl'

                // Set up kubeconfig
                withCredentials([file(credentialsId: 'your-kubeconfig-credentials-id', variable: 'KUBECONFIG')]) {
                    sh 'mv ${KUBECONFIG} kubeconfig.yaml'
                }
            }
        }

        stage('Create Namespace') {
            steps {
                sh "kubectl create namespace monitoring"
            }
        }

        stage('Deploy Prometheus') {
            steps {
                sh "./kubectl apply -f ${WORKSPACE}/values.yaml -n ${NAMESPACE}"
            }
        }

        stage('Clean Up') {
            steps {
                // Clean up temporary files or resources if needed
                sh 'rm kubeconfig.yaml'
                sh 'rm kubectl'
            }
        }
    }
}
