pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /var/jenkins_home/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            ws("/var/jenkins_workspaces/prototype"){
                steps {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }
    }
}