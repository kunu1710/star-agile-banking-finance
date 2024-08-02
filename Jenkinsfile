pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-pwd'
        SSH_CREDENTIALS_ID = 'ansible-ssh-key'
        ECR_REGION = 'ap-south-1'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sumeeth07/star-agile-banking-finance.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    sh 'docker images'
                    docker.build('sumith596/financeme:latest')
                    sh 'docker images'
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image('sumith596/financeme').push('latest')
                    }
                }
            }
        }
        stage('Run the container with port mapping') {
            steps {
                sh '''
        if [ "$(docker ps -q -f name=financeapp_new)" ]; then
            docker stop financeapp_new
            docker rm financeapp_new
        fi
        docker run -dt -p 8081:8081 --name financeapp_new sumith596/financeme:latest
        '''
            }
        }
        stage('Deploy with Ansible') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: SSH_CREDENTIALS_ID, keyFileVariable: 'SSH_KEY')]) {
                    ansiblePlaybook(
                        playbook: 'ansible-playbook.yml',
                        inventory: 'hosts',
                        extras: '--key-file=${SSH_KEY}'
                    )
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
