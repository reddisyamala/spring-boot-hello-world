pipeline {
  agent any
    stages {
        stage("Run Cross Platofroms") {
            parallel {
                stage("Build on Linux"){
                    agent {
                        label 'linux-dev'
                    }
                    steps {
                        echo "Building on Linux"
                    }
                }
                stage("Build on windows"){
                    agent {
                        label 'windows-dev'
                    }
                    steps {
                        echo "Building on windows"
                    }
                }
                stage("Build on Mac"){
                    steps {
                        echo "Building on Mac"
                    }
                }
            }
        } 
    }
}