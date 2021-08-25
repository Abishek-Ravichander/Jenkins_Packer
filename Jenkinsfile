pipeline {
environment {
        
        AWS_DEFAULT_REGION = "us-east-1"
    }
agent  any
stages {
         stage('Vault - AWS connection check') {
            steps {
                withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                       sh '''
                        aws --version
                        aws ec2 describe-instances
                        '''
}
            }
        }
        stage('checkout') {
            steps {
                 script{

                        
                            git "https://github.com/Abishek-Ravichander/Jenkins_Packer.git"
                        
                    }
                }
            }
                            
        stage('Plan') {
            steps {
                     withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                bat 'cd&cd terraform/Jenkins_Packer & packer init .'
                bat 'cd&cd terraform/Jenkins_Packer & packer fmt .'
                bat "cd&cd terraform/Jenkins_Packer & packer validate ."
                
                     }
            }
        }
       

        stage('Apply') {
            steps {
                    withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                bat "cd&cd terraform/Jenkins_Packer & packer build aws-ubuntu.pkr.hcl"
                    }
            }
        }
        
        }
   }
