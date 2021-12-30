pipeline {
    agent any
    
    environment {
        GIT_URL = 'https://github.com/consultantben/DevOps-project.git'
        TOMCAT_URL    = 'http://34.220.46.106:8080/'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git "${GIT_URL}"
            }
        }
           stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('http://34.208.44.68:9000') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
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
