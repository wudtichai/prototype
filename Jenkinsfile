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
            agent any
            steps {
                script {
                    def project = 'development-191208'
                    def appName = 'prototype'
                    def credentialsId = 'development-191208-grc-credectials'
                    
                    checkout scm
                    docker.withRegistry("https://gcr.io", "gcr:${credentialsId}") {
                        def customImage = docker.build("${project}/${appName}")
                        /* Push the container to the custom Registry */
                        customImage.push("dev-${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}