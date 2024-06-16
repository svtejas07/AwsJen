pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'  // Set your preferred AWS region
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: "${env.GITHUB_REPO}", credentialsId: "${env.GITHUB_CREDENTIALS_ID}"]]])
                }
            }
        }

        stage('Validate CloudFormation Template') {
            steps {
                script {
                    sh 'aws cloudformation validate-template --template-body file://template.yaml'
                }
            }
        }

        stage('Deploy CloudFormation Stack') {
            steps {
                script {
                    sh '''
                        aws cloudformation deploy \
                            --template-file template.yaml \
                            --stack-name my-stack \
                            --capabilities CAPABILITY_NAMED_IAM
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh 'aws cloudformation delete-stack --stack-name my-stack'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

