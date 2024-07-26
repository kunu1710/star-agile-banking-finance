pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/sumeeth07/star-agile-banking-finance.git/'
                 echo 'github url checkout'
            }
        }
        stage('Compiling the code'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('code test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('mvn QA'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package the code'){
            steps{
                sh 'mvn package'
            }
        }
        stage('build the image'){
          steps{
               sh 'docker build -t bankimage .'
           }
         }
        stage('Run the container with port mapping'){
            steps{
                sh 'docker run -dt -p 8081:8081 --name c02 bankimage'
            }
        }   
    }
}
