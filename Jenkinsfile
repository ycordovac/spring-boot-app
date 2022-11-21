def versionPom=""
pipeline{
	agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: alledovalero/jenkins-nodo-nodejs-bootcamp:1.0
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-socket-volume
    securityContext:
      privileged: true
  volumes:
  - name: docker-socket-volume
    hostPath:
      path: /var/run/docker.sock
      type: Socket
    command:
    - sleep
    args:
    - infinity
'''
            defaultContainer 'shell'
        }
    }

	environment {
		DOCKERHUB_CREDENTIALS=credentials("yandihlg")
        DOCKER_IMAGE_NAME="yandihlg/bootcamp"
    }
	
	stages{
        
		stage("Get version") {
            steps {
                script {
                    versionPom = "${pom.version}"                        
                }
            }
        }
		stage("build image"){
			steps{
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
				sh "docker build -t $DOCKER_IMAGE_NAME:${versionPom} ."
            	sh "docker push $DOCKER_IMAGE_NAME:${versionPom}"
                sh "docker build -t $DOCKER_IMAGE_NAME:latest ."
            	sh "docker push $DOCKER_IMAGE_NAME:latest"
			}
		}
        stage("Deploy to K8s"){
            steps{
		sh "rm -rf configuracion"
                sh "git clone https://github.com/ycordovac/kubernetes-helm-docker-config.git configuracion --branch demo-java"
                sh "kubectl apply -f configuracion/kubernetes-deployments/spring-boot-app/deployment.yaml --kubeconfig=configuracion/kubernetes-config/config"
            }
        }
	}
    post{
        always{
            sh "docker logout"
        }
    }
}
