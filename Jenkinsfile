pipeline {
    agent any

    stages {
        stage('Checkout Code from GitHub') {
            steps {
                git url: 'https://github.com/kunu1710/star-agile-banking-finance.git'
                echo 'GitHub URL checked out'
            }
        }
        stage('Compile Code') {
            steps {
                echo 'Starting compilation'
                sh 'mvn compile'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('QA Check') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t suvakantanayak/banking:latest .'
                sh 'docker run -itd -p 8081:8081 suvakantanayak/banking:latest'
            }
        }
        stage('Log in to DockerHub and push the image to dockerhub') {
            steps{
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                sh 'docker login -u suvakantanayak -p ${dockerhubpass}'
                sh 'docker push suvakantanayak/banking:latest'
                    }
            }
        }
        stage('Deployment stage using ansible') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultTmpPath: ''
            }
        }
    }
}
    
