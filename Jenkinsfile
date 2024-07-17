pipeline {
    agent any
    parameters {    
        choice choices: ['apply', 'destroy'], description: 'Do you want to apply or destroy resources?', name: 'action'
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Checkout Github Repository'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nehapatil-coditas/jenkins-ci-terraform-pipeline.git']])
            }
        }
        stage('Version'){
            steps{
                bat 'terraform --version'
            }
        }
        stage('Init'){
            steps{
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    bat 'terraform init'
                }
                
            }
        }
        stage('Validate') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    bat 'terraform validate'
                }
            }
        }
        stage('Format') {
            steps {
                bat 'terraform fmt'
            }
        }        
        stage('Plan') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    bat 'terraform plan'
                }
            }
        }
        stage('Apply') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    script {
                        if(params.action == 'destroy'){
                            bat 'terraform destroy -auto-approve'
                        } else {
                            bat 'terraform apply -auto-approve'
                        }
                    }
                }
            }
        }
      
        
    }
}
