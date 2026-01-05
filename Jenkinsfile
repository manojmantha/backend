
pipeline {
    agent { label 'AGENT-1' }
    environment { 
        PROJECT = 'expense'
        COMPONENT = 'backend'
        appVersion = ''
    }
    options {
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }
    // parameters{
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
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
