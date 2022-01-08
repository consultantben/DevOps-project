pipeline {
    agent any
    
    environment {
        GIT_URL = 'https://github.com/consultantben/DevOps-project.git'
        TOMCAT_URL    = 'http://54.149.42.15:8080/'
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
              withSonarQubeEnv('sonar') {
                sh 'mvn -f SampleWebApp/pom.xml clean package sonar:sonar'
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
        stage('Deploy to JFrog') {
            steps {
                rtUpload (
                    serverId: 'my-jfrog',   #Server ID that was configured on Jenkins
                    spec: '''{​​​​​
                          "files": [
                            {​​​​​​​​​​​​​​​​​​​​​​​​​​
                              "pattern": "**/*.war",
                              "target": "my-repo/".  # Target is a repository that we created on JFrog
                            }​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​
                        ]
                    }​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​''',
                )
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
