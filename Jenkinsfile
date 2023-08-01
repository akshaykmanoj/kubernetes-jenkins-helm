pipeline {
    agent any
    environment {
        build_number = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID="409######793"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="registration-helm"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_key_helm', url: 'github repo url']])
            }
        }
        stage('Python Build') {
            steps {
                //bat 'pip install -r ./requirements.txt'
                dir('./registattionproject'){
                    bat 'python settings.py build'
                }
            }
        }
        stage('Unit Test') {
            steps {
                dir('./registration/app1') {
                    bat 'python ../../manage.py test'
                }
            }
        }

        stage('docker login build push and logout')  {
            steps{
                withCredentials([string(credentialsId: 'username', variable: 'docker-pwd')]) {
                    bat 'docker login -u username -p %docker-pwd%'
                    bat "docker build -t  docker-imagename:${env.BUILD_NUMBER} . "
                    bat "docker push docker-imagename:${env.BUILD_NUMBER}"
                    bat 'docker logout'
                }
            }
        }
        
        stage('replace helmChart tag ') {
            steps {
                //bat "sed -i 's|docker-imagename:v5|docker-imagename:${env.BUILD_NUMBER}|g' ./registration-helm/values.yaml"
                bat """
                powershell.exe -Command "((Get-Content -Path './registration-helm/values.yaml') -replace 'docker-imagename:v5', 'docker-imagename:${env.BUILD_NUMBER}') | Set-Content -Path './registration-helm/values.yaml'"
                """
            }
        }
        
        stage('helm package ') {
            steps {
                //bat 'helm package registration-helm'
                bat 'C:\\windows-amd64\\helm package E:\\Project\\Registration\\registrationproject\\registration-helm'

            }
        }
        stage('Logging into AWS ECR & push helm chart to ecr') {
            steps {
                script {
                 withCredentials([aws(credentialsId: 'ecr-credential', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {   
                    // bat 'aws ecr get-login-password --region us-east-1 | helm registry login --username AWS --password-stdin 409######793.dkr.ecr.us-east-1.amazonaws.com'
                     bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws" ecr get-login-password --region us-east-1 | C:\\windows-amd64\\helm registry login --username AWS --password-stdin 409######793.dkr.ecr.us-east-1.amazonaws.com'
                     // bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws" ecr create-repository --repository-name registration-helm --region us-east-1'
                     bat "C:\\windows-amd64\\helm push  registration-helm-0.1.0.tgz oci://409######793.dkr.ecr.us-east-1.amazonaws.com"
                     bat "del registration-helm-0.1.0.tgz"
                     //bat '"C:\\Program Files\\Amazon\\AWSCLIV2\\aws" ecr delete-repository --repository-name registration-helm --region us-east-1 --force'
                 }
                }
            }
        }
         stage('pass buildnumber to another pipeline') {
            steps {
                build job: 'deploy-pipe', parameters: [string(name: 'build_number', value: "${build_number}")]
            }
        }
    }
}
