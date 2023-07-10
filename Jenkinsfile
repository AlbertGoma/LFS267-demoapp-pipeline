pipeline {
    environment {
        registry = "albertgoma/jenkinstest"
        registryCredential = 'docker-hub-id'
        dockerImage = ''
    }
    agent any 
    stages {
        stage('SCM') {
            steps {
                git 'https://github.com/dhgautam/docker-test.git'
            }
        }
        stage('Build') {
            steps {
                echo "Running build in branch ${env.BRANCH_NAME} ...."
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        //stage('Test') {
        //    steps {
        //        echo "Running tests in branch ${env.BRANCH_NAME} ...."
        //    }
        //}
        //stage('Publish s3') {
        //    steps {
        //        withAWS(credentials:'jenkins-role-s3', region:'eu-north-1') {
        //           s3Upload(bucket:"ci-artifacts-jenkins-lfs267", includePathPattern: '**/.jar', pathStyleAccessEnabled: true, sseAlgorithm:'AES256')
        //        }
        //    }
        //}

        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

    }
}

