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
                sh 'mvn -B -DskipTests clean package'      
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build Docker Image') { 
            def project = 'development-191208'
            def appName = 'prototype'
            def registryServer = "gcr.io/${project}/${appName}"
            def credentialsId = 'development-191208'
            steps {
                checkout scm
                docker.withRegistry("${registryServer}", "${credentialsId}") {
                    def customImage = docker.build("${appName}:dev-${env.BUILD_ID}")
                    /* Push the container to the custom Registry */
                    customImage.push()
                }
            }
        }
    }
}