pipeline {
   agent any
    environment {
        def scannerHome = tool 'sonarScanner5.0.1'
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
                withSonarQubeEnv('mysonarQube') {
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

         stage("Upload Artifacts") {
            steps {
                rtServer (
                    id: 'artifactor-dev',
                    url: 'http://683b06656b2c.mylabserver.com/artifactory',
                    username: 'admin',
                    password: 'Admin@123'
                    // credentialsId: 'ccrreeddeennttiiaall'
                    bypassProxy: true
                    timeout = 300
                )

                rtUpload (
                    serverId: "artifactor-dev",
                    spec:
                        """{
                        "files": [
                            {
                            "pattern": "targe/*.jar",
                            "target": "example-repo-local/springbootapp/"
                            }
                        ]
                        }""",
                    failNoOp: true
                )


            }
        }


    }
}