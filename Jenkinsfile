pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"   // Change to your AWS region
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/snaatak-deepak/AMI.git'
            }
        }

        stage('Validate Packer Template') {
            steps {
                sh "packer validate template.json"
            }
        }

        stage('Build AMI') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',
                                  credentialsId: 'aws-cred']]) {
                    sh """
                        packer build \
                          -var 'aws_region=${AWS_REGION}' \
                          -var 'aws_access_key=${AWS_ACCESS_KEY_ID}' \
                          -var 'aws_secret_key=${AWS_SECRET_ACCESS_KEY}' \
                          template.json
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ AMI build completed successfully!"
        }
        failure {
            echo "❌ AMI build failed!"
        }
    }
}
