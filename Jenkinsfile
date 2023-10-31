pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t prakhards98/jenkins-react .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push prakhards98/jenkins-react'
      }
    }

    stage('Deploying React.js container to K8s') {
      steps {
        // script {
        //   kubernetesDeploy(configs: "deployment.yml", "loadbalancer.yml")
        // }
        // sh 'kubectl apply -f deployment.yml'
         withKubeConfig([credentialsId: 'kubeconfig']) {
             sh '/usr/local/bin/kubectl apply -f deployment.yml --kubeconfig=~/.kube/config'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
