pipeline {
   agent any
    
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
    }
}

 

       