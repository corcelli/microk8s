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
                    #def dockerHome = tool 'docker1'
                    #env.PATH = "${dockerHome}/bin:${env.PATH}"
                    appName = "app"
                    tag = "latest"
                    registryHost = "10.152.183.135:30400/"
                    imageName = "${registryHost}${appName}:${tag}"
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
        

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "manifest.yaml", kubeconfigId: "kubeconfig.yaml")
        }
      }
    }
}
}
