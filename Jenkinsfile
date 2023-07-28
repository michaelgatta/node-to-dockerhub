pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', credentialsId: 'gitcheckout', url: 'https://github.com/michaelgatta/dockermike'
            }
        }
        stage("Docker Build") {
            steps{
                sh "docker build -t michaelgat/works-jenkin_server:latest ."
            }
        }
        stage("Authenticate") {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                echo "Login complete..."
            }
        }
        stage("Push to Registry") {
            steps{
                script {
                    sh 'docker tag michaelgat/works-jenkin_server docker.io/michaelgat/works-jenkin_server:$BUILD_ID'
                    sh 'docker push michaelgat/works-jenkin_server:$BUILD_ID'
                }
            }
        }
        stage("Cleanup") {
            steps{
                sh 'docker rmi -f $(docker image ls -aq)'
                sh 'docker logout'
            }
        }
    }
}
