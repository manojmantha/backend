
pipeline {
    agent { label 'AGENT-1' }
    environment { 
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
        ACC_ID = '533046675162'
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {
        stage('App Version') {
            steps {
               script{
                 def jsonPackage = readJSON file: 'package.json'
                 appVersion = jsonPackage.version
                 echo "App Version : $appVersion"
               }
            }
        }
        
        stage('Install dependencies') {
            steps {
                sh """
                    npm install
                """
            }
        }

        stage('docker build') {
            steps {
                withAWS(region: 'us-east-1', credentials: 'aws-creds') {
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:$appVersion .

                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:$appVersion
                    """
                }
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'I will run when pipeline is failed'
        }
        success { 
            echo 'I will run when pipeline is success'
        }
    }
}
