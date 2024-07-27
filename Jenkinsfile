pipeline {
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-credentials'
        DOCKER_CREDENTIALS_ID = '5a2be959-9281-46bf-8699-00c708e95f9c'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: "${GIT_CREDENTIALS_ID}", url: 'https://github.com/jinkasaru123/python-webapp.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    docker.build('jinkasaru123/python-webapp')
                }
            }
        }

        stage('Test') {
            steps {
                sh 'python -m unittest discover'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        docker.image('jinkasaru123/python-webapp').push('latest')
                    }
                }
                kubernetesDeploy configs: 'k8s-deployment.yaml', kubeConfig: [path: '']
            }
        }
    }
}
