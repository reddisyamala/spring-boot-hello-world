pipeline {
   agent any
    environment {
        def scannerHome = tool 'mysonarQube'
    }
    stages {
        stage("Build"){
            steps {
               script {
                sh "mvn install"
                sh "mv target/*.jar target/spring-boot-2-hello-world-1.0.2-SNAPSHOT-${BUILD_NUMBER}.jar"
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
              timeout(time: 2, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
    }
}
 

       