pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /var/jenkins_home/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                ws('/var/jenkins_workspaces') {
                    sh 'mvn -B -DskipTests clean package' 
                }
            }
        }
    }
}