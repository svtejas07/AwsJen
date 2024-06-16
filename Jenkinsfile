pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'  // Set your preferred AWS region
    }

    stages {
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
    }

    post {
        success {
            echo 'Stack deployed successfully.'
        }
        failure {
            echo 'Stack deployment failed.'
        }
    }
}
