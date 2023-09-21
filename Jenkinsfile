pipeline {
   agent any
   stages {
        
        stage("Build"){
            steps {
               script {
                  sh "ls -lrt"
                  sh "mvn install"
               }
            }
        }

        stage("Unit-Test"){
            steps {
                echo "Running Unit-Tests ....."
            }
        }
    }
}