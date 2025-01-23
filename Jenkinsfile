pipeline {
    agent any

    environment {
        GOOGLE_APPLICATION_CREDENTIALS = credentials('gke-credentials') // Jenkins credential ID for GCP service account
    }

    stages {
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/VaibhavchavanDevOps/terraform_script.git'
		    }
        }
        stage('Setup') {
            steps {
                script {
                    // Install Google Cloud SDK
                    //sh 'curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-367.0.0-linux-x86_64.tar.gz'
                    //sh 'tar -xf google-cloud-sdk-367.0.0-linux-x86_64.tar.gz'
                    //sh './google-cloud-sdk/install.sh -q'
                    //sh 'source ./google-cloud-sdk/path.bash.inc'
                    
                    // Authenticate with Google Cloud
                    sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                    sh 'gcloud config set project poc-cluster-443705'
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Create Terraform plan
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply Terraform plan
                    sh 'terraform apply -auto-approve tfplan'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
