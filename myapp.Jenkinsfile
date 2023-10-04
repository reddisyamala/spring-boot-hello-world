pipeline {
   agent any
    environment {
        def scannerHome = tool 'sonarscanner'
    }
    stages {
        stage("Build"){
            steps {
               script {
                sh "mvn install"
               }
            }
        }

        stage("Unit-Test"){
            steps {
                script {
                    sh "mvn test"
                }
            }
        }

        stage("Code Analysis"){
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh """ ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=spring-boot-hello-world \
                    -Dsonar.projectName=spring-boot-hello-world \
                    -Dsonar.sources=. \
                    -Dsonar.java.binaries=target/classes \
                    -Dsonar.sourceEncoding=UTF-8
                    """
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 300, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
    }
}
 

       