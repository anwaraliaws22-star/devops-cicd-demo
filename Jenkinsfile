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
                credentialsId: 'anwaraliaws22',
                usernameVariable: 'DOCKER_USERNAME',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            bat '''
    echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
    if errorlevel 1 exit /b 1

    docker tag devops-cicd-demo:1.0 %DOCKER_USERNAME%/devops-cicd-demo:1.0

    docker push %DOCKER_USERNAME%/devops-cicd-demo:1.0
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
