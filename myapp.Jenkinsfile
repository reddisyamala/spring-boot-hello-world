pipeline {
   agent any
    environment {
        def scannerHome = tool 'sonarScanner5.0.1'
    }
   stages {
        stage("Build"){
            steps {
               script {
                sh "rm -rf target/*.jar"
                sh "mvn install"
                sh "mv target/*.jar target/spring-boot-2-hello-world-1.0.2-SNAPSHOT-${BUILD_NUMBER}.jar"
                sh "printenv"
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

        //  stage("Upload Artifacts") {
        //     steps {
        //         rtServer (
        //             id: 'jfrogdev',
        //             url: 'http://683b06656b2c.mylabserver.com/artifactory/',
        //             username: 'admin',
        //             password: 'Admin@123',
        //             // credentialsId: 'ccrreeddeennttiiaall'
        //             timeout = 300
        //         )

        //         rtUpload (
        //             serverId: "jfrogdev",
        //             spec:
        //                 """{
        //                 "files": [
        //                     {
        //                     "pattern": "targe/*.jar",
        //                     "target": "example-repo-local/springbootapp/"
        //                     }
        //                 ]
        //                 }""",
        //             failNoOp: true
        //         )


        //     }
        // }

        stage("Upload Artifacts"){
            steps{
                
                rtServer (
                        id: 'jfrog-server',
                        url: 'http://683b06656b2c.mylabserver.com/artifactory/',
                        // If you're using username and password:
                        username: 'admin',
                        password: 'Admin@123',
                        // If you're using Credentials ID:
                        // credentialsId: 'ccrreeddeennttiiaall',
                        // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                        // bypassProxy: true,
                        // Configure the connection timeout (in seconds).
                        // The default value (if not configured) is 300 seconds: 
                        timeout: 300
                )
                rtUpload (
                    serverId: 'jfrog-server',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "target/*.jar",
                            "target": "example-repo-local/spring-boot-hello-world/"
                            }
                        ]
                    }''',
                )    
            }
        } 


    }
}