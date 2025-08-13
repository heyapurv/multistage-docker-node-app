pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "heyapurv/node-app"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh """
                        docker rm -f node-app || true
                        docker run -d --name node-app -p 3000:3000 ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
