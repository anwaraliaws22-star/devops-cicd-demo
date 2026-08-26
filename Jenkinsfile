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

        stage('Docker Build') {
            steps {
                bat 'docker build -t devops-cicd-demo:1.0 .'
            }
        }

        stage('Docker Push') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-jenkins',
                usernameVariable: 'DOCKER_USERNAME',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            bat '''
                echo Docker username: %DOCKER_USERNAME%

                powershell -NoProfile -Command "$p=$env:DOCKER_PASSWORD; Write-Host ('Password received by Jenkins: ' + ($p.Length -gt 0) + ', length=' + $p.Length)"

                echo Logging into Docker Hub...

                powershell -NoProfile -Command "$env:DOCKER_PASSWORD | docker login -u $env:DOCKER_USERNAME --password-stdin"
            '''
        }
    }
}

        stage('Deploy') {
            steps {
                bat 'docker rm -f devops-cicd-demo-container 2>nul || echo No existing container'
                bat 'docker run -d --name devops-cicd-demo-container -p 8081:8080 devops-cicd-demo:1.0'
            }
        }
    }
}
