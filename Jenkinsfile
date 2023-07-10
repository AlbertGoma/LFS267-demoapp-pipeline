pipeline {
    agent any 
    stages {
        stage('Build') {
            steps {
                echo "Running build in branch ${env.BRANCH_NAME} ...."
            }
        }
        stage('Test') {
            steps {
                echo "Running tests in branch ${env.BRANCH_NAME} ...."
            }
        }
        stage('Publish s3') {
            steps {
                withAWS(credentials:'jenkins-role-s3', region:'eu-north-1') {
                    s3Upload(bucket:"ci-artifacts-jenkins-lfs267", includePathPattern: '**/.jar', pathStyleAccessEnabled: true, sseAlgorithm:'AES256')
                }
            }
        }
    }
}

