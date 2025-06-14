pipeline {
    agent { label 'STAGE-1'}
    environment {
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
        ACC_ID = '381491879282'
        environment = ''

    }
    options {
        disableConcurrentBuilds()
        timeout(time:30 , unit: 'MINUTES')
    }
    parameters {
        string(name: 'version', description: 'enter the app version')
        choice(name: 'deploy_to', choices: ['dev', 'qa', 'prod'], description: 'Pick something')
    }

    stages{
        stage('setup Environment') {
            steps {
                script {
                    appVersion = params.version
                    environment = params.deploy_to
                    sh """
                        echo "$appVersion"
                        echo "$environment"
                    """
                }
            }
        }

        stage ('Deploy') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'aws-cred') {
                        sh """
                            aws eks update-kubeconfig --region us-east-1 --name expense-dev
                            kubectl get nodes
                            cd helm/
                            sed -i 's/IMAGE_VERSION/${appVersion}/g' values-${environment}.yaml
                            cat values-${environment}.yaml
                            helm upgrade --install $COMPONENT -n expense -f values-${environment}.yaml .
                        
                        """
                    }
                }
            }
        }
    }
}