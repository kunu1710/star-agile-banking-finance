pipeline {
    agent any

    stages {
        stage('Checkout Code from GitHub') {
            steps {
                git url: 'https://github.com/sumeeth07/star-agile-banking-finance.git'
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
                sh 'docker build -t sumith596/finance:latest .'
                sh 'docker run -itd -p 8083:8081 sumith596/financeme:latest'
            }
        }
        stage('Log in to DockerHub and push the image to dockerhub') {
            steps{
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                sh 'docker login -u sumith596 -p ${dockerhubpass}'
                sh 'docker push sumith596/financeme:latest'
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
    
