node {
	def application = "todolist"
	def dockerhubaccountid = "charannaik"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
	}
	}

	stage('Deploy') {
		// sh ("docker run -dp 3000:3000 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
		sh '''if [  containerid="$(docker ps -a | grep ${dockerhubaccountid}/${application} | docker ps -q)" ]; then
				("docker stop $containerid")
				("docker rm $containerid")
  			else
				("docker run --rm -dp 3000:3000 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
		fi'''
	}
	
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }
}
