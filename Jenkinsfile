pipeline(
	agent(
		label "node-java"
	)
	
	stages(
		stage["build"](
			step(
				sh "mvn clean  package -DskipTest"
			)
		)
	)
)
