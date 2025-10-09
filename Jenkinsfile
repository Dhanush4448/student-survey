pipeline {
  agent any
  environment {
    IMAGE   = "dhanush853/studentsurvey"       // your Docker Hub repo
    TAG     = "${env.BUILD_NUMBER}"            // unique tag per build
    K8S_NS  = "default"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build image (:build# and :latest)') {
      steps {
        sh "docker build -t ${IMAGE}:${TAG} -t ${IMAGE}:latest ."
      }
    }

    stage('Push to Docker Hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                          usernameVariable: 'DH_USER',
                                          passwordVariable: 'DH_PASS')]) {
          sh 'echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin'
          sh "docker push ${IMAGE}:${TAG}"
          sh "docker push ${IMAGE}:latest"
        }
      }
    }

    stage('Render K8s manifest with new tag') {
      steps {
        sh 'env IMAGE=${IMAGE} TAG=${TAG} envsubst < deployment.tmpl.yaml > deployment.rendered.yaml'
        sh 'echo "--- Rendered manifest ---"; head -n 40 deployment.rendered.yaml || true'
      }
    }

    stage('Apply to Kubernetes') {
      steps {
        sh "kubectl -n ${K8S_NS} apply -f deployment.rendered.yaml"
        sh "kubectl -n ${K8S_NS} rollout status deployment/studentsurvey-deployment --timeout=180s"
      }
    }
  }
  post {
    always {
      sh "kubectl -n ${K8S_NS} get deploy,svc,pods -o wide || true"
    }
  }
}
