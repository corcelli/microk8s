pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/corcelli/microk8s.git', branch:'main'
      }
    }

    
      stage("Build image") {
            steps {
                script {
                    appName = "app"
                    tag = "latest"
                    registryHost = "registry-es.services.netextreme.com.br/"
                    imageName = "${registryHost}${appName}"
                    customImage = docker.build("${imageName}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    customImage.push()
                    }
                }
            }

    
    stage ('Deploy') {
    steps{
        sshagent(['ssh-key']) {
            sh 'kubectl apply -f manifest.yaml'
            sh 'kubectl rollout restart deployment app'
        }
    }
}

  
}

}
