pipeline{
	agent{
		label "nodo-java"
	}
	
	stages{
		stage("build"){
			steps{
				sh "mvn clean package -DskipTest"
			}
		}
	}
}
