pipeline {
    agent any

    environment {
        DOCKERHUB = "manavmain/jenkins-docker-ci-cd-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/manavbhatiaa/jenkins-docker-ci-cd-demo.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKERHUB:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKERHUB:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                   docker rm -f jenkins-demo || true
                   docker run -d --name jenkins-demo -p 3000:3000 $DOCKERHUB:latest
                '''
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
