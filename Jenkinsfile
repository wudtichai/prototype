node {
  def project = 'development-191208'
  def appName = 'prototype'
  def imageTag = "gcr.io/${project}/${appName}:${env.POM_VERSION}-${env.BUILD_NUMBER}"
  def environment = 'dev'

  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }
  }

  stages {
    stage('Build image') {
      steps {
        sh 'mvn clean install package'
        sh 'docker build -t ${imageTag} .'
      }
    }
    
    stage('Push image to registry') {
      steps {
        sh("gcloud docker -- push ${imageTag}")
      }
    }

    stage('Deploy Application') {
      steps {
        // Create namespace if it doesn't exist
        sh("kubectl get ns ${environment} || kubectl create ns ${environment}")
        // Don't use public load balancing for development branches
        sh("sed -i.bak 's#${appName}-image#${imageTag}#' ./k8s/${environment}/*yaml")
        sh("kubectl --namespace=${environment} apply -f k8s/${environment}/")
        echo 'To access your environment run `kubectl proxy`'
        echo "Then access your service via http://localhost:8001/api/v1/proxy/namespaces/${environment}/services/${appName}:8080/"
      }
    }
  }
}