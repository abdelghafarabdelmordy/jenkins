
pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        TF_VAR_region = 'us-east-1'
        TF_VAR_vpc_cidr_block = '10.0.0.0/16'
        TF_VAR_subnet_cidr_blocks = '["10.0.1.0/24", "10.0.2.0/24"]'
        TF_VAR_availability_zones = '["us-east-1a", "us-east-1b"]'
    
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/abdelghafarabdelmordy/jenkins.git'
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                                     string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh '''
                            export TF_VAR_aws_access_key=$AWS_ACCESS_KEY_ID
                            export TF_VAR_aws_secret_key=$AWS_SECRET_ACCESS_KEY
                            terraform plan
                        '''
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    def userInput = input(
                        message: 'Do you want to run Terraform apply? Select "Proceed" to apply changes, or "Abort" to skip.',
                        parameters: [
                            choice(choices: 'Proceed\nAbort', description: 'Choose whether to proceed with Terraform apply or abort the job.', name: 'ACTION')
                        ]
                    )

                    if (userInput == 'Proceed') {
                        echo 'Running Terraform apply...'
                        withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                                         string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                            sh '''
                                export TF_VAR_aws_access_key=$AWS_ACCESS_KEY_ID
                                export TF_VAR_aws_secret_key=$AWS_SECRET_ACCESS_KEY
                                terraform apply -auto-approve
                            '''
                        }
                    } else {
                        echo 'Skipping Terraform apply.'
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                def userInput = input(
                    message: 'Do you want to run Terraform destroy? Select "Proceed" to run destroy, or "Abort" to skip.',
                    parameters: [
                        choice(choices: 'Proceed\nAbort', description: 'Choose whether to proceed with Terraform destroy or abort the job.', name: 'ACTION')
                    ]
                )

                if (userInput == 'Proceed') {
                    echo 'Running Terraform destroy...'
                    withCredentials([string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                                     string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh '''
                            export TF_VAR_aws_access_key=$AWS_ACCESS_KEY_ID
                            export TF_VAR_aws_secret_key=$AWS_SECRET_ACCESS_KEY
                            terraform destroy -auto-approve
                        '''
                    }
                } else {
                    echo 'Skipping Terraform destroy.'
                }
            }
        }
    }
}
