pipeline {
    agent any
    
    tools {
         nodejs 'nodejs18'
    }
    
    options {
        buildDiscarder logRotator(numToKeepStr: '1')
    }
    
    stages {
        stage('Git Repo Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/techpentest/nodejs-todo-app.git'
            }
        }
        
        stage('NPM Install') {
            steps {
                     sh 'npm install'
            }
        }
        
        //  stage('NPM Test') {
        //     steps {
        //         sh '''
        //         chmod +x ./node_modules/.bin/jest
        //         npm run test
        //         '''
        //     }
        // }
        
        stage('Phase1-Snyke code analysis') {
            steps {
                echo 'Testing...'
                snykSecurity(
                              snykInstallation: 'snyk',
                              snykTokenId: 'snyk-api-token' ,
                              monitorProjectOnBuild: true,
                              failOnIssues: true
                )
            }
        }
        
    }
    
    post {
            aborted {
                echo 'pipeline has aborted by user, please check the reason.'
            }
            success {
                echo 'Application is successfully deployed, !!'
            }
            failure {
                echo 'Pipeline is failed, Please check the log'
            }
     }
}
