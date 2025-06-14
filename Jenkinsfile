pipeline {
    agent { label 'STAGE-1'}
    environment {
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
        ACC_ID = '381491879282'
    }
    options {
        disableConcurrentBuilds()
        timeout(time:30 , unit: 'MINUTES')
    }
    parameters {
        string(name: 'version', description: 'enter the app version')
    }

    stages{
        stage ('Deploy') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-cred') {
                        sh """
                            aws eks update-kubeconfig --region us-east-1 --name expense-dev
                            kubectl get nodes
                        """
                    }
                }
            }
        }
    }
}