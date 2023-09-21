pipeline {
   agent any
   stages {
        
        stage("Build"){
            steps {
               script {
                  sh "ls -lrt"
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