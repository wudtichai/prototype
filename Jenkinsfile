pipeline {
    agent any
    stages {
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /var/jenkins_home/.m2:/root/.m2' 
                }
            }
            steps {
                sh 'mvn -B clean package'      
            }
            post {
                success {
                    sh 'cp -r target ../workspace/target'  
                }
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Push Image') { 
            steps {
                script {
                    def project = 'development-191208'
                    def appName = 'prototype'
                    def credentialsId = 'development-191208-grc-credectials'
                    def customImage = docker.build("${project}/${appName}")
                    docker.withRegistry("https://asia.gcr.io", "gcr:${credentialsId}") {
                        customImage.push("dev-${env.BUILD_ID}")
                    }
                }
            }
        }
    }
}