pipeline {
    agent any

    stages {
        stage('Checkout scm') {
            steps { checkout scm}
        }

        stage('build') {
            steps {sh 'npm install'}
        }
    }
}