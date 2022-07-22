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
                    registryHost = "registry.services.sitac.com.br/"
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
        sshagent(credentials : ['use-the-id-from-credential-generated-by-jenkins']) {
            sh 'kubectl apply -f manifest_v1.yaml'
            sh 'kubectl rollout restart deployment app'
        }
    }
}

  
}

}