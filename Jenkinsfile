pipeline(
	agent(
		label: "nodo-java"
	)
	
	stages(
		stage["build"](
			step(
				sh "mvn clean  package -DskipTest"
			)
		)
	)
)
