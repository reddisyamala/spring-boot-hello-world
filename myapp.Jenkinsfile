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
              timeout(time:2 , unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
        stage("Upload Artifacts"){
            steps{
                
                rtServer (
                        id: 'jfrog-server',
                        url: 'http://192.168.29.189:8082/artifactory',
                        // If you're using username password
                        username: 'admin',
                        password: '$Uriya@32',
                        timeout: 300
                )
                rtUpload (
                    serverId: 'jfrog-server',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "target/*.jar",
                            "target": "example-repo-local/"
                            }
                        ]
                    }''',
                )    
            }
        } 
    }
}
 

       