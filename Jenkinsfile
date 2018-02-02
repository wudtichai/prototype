pipeline {
    agent any
    tools { 
        maven 'Maven 3.5.2' 
        jdk 'jdk8u162' 
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn -B clean package'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        // stage('Build') { 
        //     agent {
        //         docker {
        //             image 'maven:3-alpine' 
        //             args '-v /var/jenkins_home/.m2:/root/.m2' 
        //         }
        //     }
        //     steps {
        //         sh 'mvn -B clean package'      
        //     }
        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }
        stage('Push Image') { 
            steps {
                sh 'pwd'
                script {
                    def project = 'development-191208'
                    def appName = 'prototype'
                    def credentialsId = 'development-191208-grc-credectials'
                    
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