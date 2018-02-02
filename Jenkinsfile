pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /var/jenkins_workspaces/.m2:/var/jenkins_workspaces/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    }
}