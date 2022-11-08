def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]



pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning project codebase...'
                git branch: 'main', url: 'https://github.com/Fred-Mbore/airbnb-infrastructure.git'
                sh 'ls'
            }
        } 
         stage('Verify Terraform version') {
            steps {
                echo 'Verifiying the Terraform version...'
                sh 'terraform --version'
            }
        }
        
          stage('Verify Terraform init') {
            steps {
                echo 'Initiliazing the Terraform project...'
                sh 'terraform init'
            }
        }
        
         stage('Verify Terraform validate') {
            steps {
                echo 'Code syntax checking...'
                sh 'terraform validate'
            }
        }
        
          stage('Terraform plan') {
            steps {
                echo 'Terraform plam for the dry run...'
                sh 'terraform plan'
            }
        }
        
        
         stage('checkov scan') {
            steps {
                sh """
                sudo pip3 install checkov
                checkov -d . --skip-check CKV_AWS_79
                """
            }
        }
        stage('Manual approval') {
            steps {
                input 'Approval required for deployment'
            }
        }
        
           stage('Terraform apply') {
            steps {
                echo 'Terraform apply...'
                sh 'sudo terraform apply --auto-approve'
            }
        }
    }
    
    
     post {
        always {
            echo 'I will always say Hello again!'
            slackSend channel: '#fred-devops-team', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }  
    }
}
    

   