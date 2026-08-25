pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anwaraliaws22-star/devops-cicd-demo.git'
            }
        }

        stage('Build') {
            steps {
                bat '''mvn -Dmaven.repo.local="%WORKSPACE%/.m2/repository" clean package -DskipTests'''
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
    }
}