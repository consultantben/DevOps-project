pipeline {
    agent any
    
    environment {
        GIT_URL = 'https://github.com/consultantben/DevOps-project.git'
        TOMCAT_URL    = 'http://34.221.212.37:8080/'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git "${GIT_URL}"
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-id',
                path: '',
                url: "${TOMCAT_URL}")],
                contextPath: 'myapp', 
                war: '**/*.war'
            }
        }
    }
}
