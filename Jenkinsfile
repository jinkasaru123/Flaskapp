pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials' // GitHub credentials ID
        DOCKER_CREDENTIALS_ID = '5a2be959-9281-46bf-8699-00c708e95f9c' // Docker Hub credentials ID
        DOCKER_BUILDKIT = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: "${GIT_CREDENTIALS_ID}", url: 'https://github.com/jinkasaru123/Flaskapp.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build('jinkasaru123/flaskapp')
                }
            }
        }

        stage('Test') {
            steps {
                sh 'python -m unittest discover'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image('jinkasaru123/flaskapp').push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy configs: 'k8s-deployment.yaml', kubeConfig: [path: '']
            }
        }
    }
}
