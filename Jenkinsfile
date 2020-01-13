podTemplate(containers: [
	containerTemplate(name: 'docker', image: 'docker:19.03.5', ttyEnabled: true, command: 'cat'),
	containerTemplate(name: 'helm', image: 'alpine/helm:3.0.2', ttyEnabled: true, command: 'cat')],
	volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
    node(POD_LABEL) {
	git 'https://github.com/Mozuffer/podtemplate.git'
	env.GIT_COMMIT = sh(script: "git rev-parse HEAD", returnStdout: true).trim()

        stage('Build') {
	    container('docker'){
		sh 'docker build -t uqudo.azurecr.io/node-app:${GIT_COMMIT} .'
		docker.withRegistry('https://uqudo.azurecr.io', 'uqudo-acr') {
		sh 'docker push uqudo.azurecr.io/node-app:${GIT_COMMIT}'
            } 
	   }
        }
	
	stage('Deploy') {
	  container('helm') {
	    stage('Install node-app helm chart') {
            	sh 'helm upgrade --install node-app helm/node-app --set image.tag=${GIT_COMMIT} -n default'
	   }
	}
        }
    }
}
