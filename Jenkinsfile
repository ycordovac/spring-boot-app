def versionPom = ""
pipeline{
	agent {
        node {
            label "nodo-java"
        }
    }

    environment {
        registryCredential='yandihlg'
        registryBackend = 'yandihlg/backend-demo'
    }
	stages {

        stage('Push Image to Docker Hub') {
          steps {
            script {
              dockerImage = docker.build registryBackend + ":$BUILD_NUMBER"
              docker.withRegistry( '', registryCredential) {
                dockerImage.push()
              }
            }
          }
        }

        stage('Push Image latest to Docker Hub') {
          steps {
            script {
              dockerImage = docker.build registryBackend + ":latest"
              docker.withRegistry( '', registryCredential) {
                dockerImage.push()
              }
            }
          }
        }

		stage("Deploy to K8s")
		{
			steps{
                script {
                  if(fileExists("configuracion")){
                    sh 'rm -r configuracion'
                  }
                }

				sh 'git clone https://github.com/ycordovac/kubernetes-helm-docker-config.git configuracion --branch test-implementation'
				sh 'kubectl apply -f configuracion/kubernetes-deployment/spring-boot-app/manifest.yml -n default --kubeconfig=configuracion/kubernetes-config/config'
			}
		}

	}
	post {
		always {
			sh "docker logout"
		}
	}
}