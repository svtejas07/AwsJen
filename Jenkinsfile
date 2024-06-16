pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'  // Set your preferred AWS region
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

