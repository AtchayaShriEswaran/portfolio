pipeline {
    agent any
     environment {
        DOCKERHUB_REPO = 'atchayashri/portfolio'
      }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AtchayaShriEswaran/portfolio.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_USER/portfolio .'
        
        }
        stage('Push to DockerHub') {
            steps {
withCredentials([usernamePassword(
    credentialsId: 'docker-hub-cred',
    usernameVariable: 'DOCKER_USER',
    passwordVariable: 'DOCKER_PASS'
)]) {
    sh '''
        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
    '''
                    sh 'docker push $DOCKERHUB_REPO'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}

