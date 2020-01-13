podTemplate(containers: [
	containerTemplate(name: 'docker', image: 'docker:19.03.5', ttyEnabled: true, command: 'cat'),
	containerTemplate(name: 'helm', image: 'alpine/helm:3.0.2', ttyEnabled: true, command: 'cat')],
	volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
    node(POD_LABEL) {
        stage('build') {
            git 'https://github.com/Mozuffer/podtemplate.git'
	    container('docker'){
		sh 'docker build -t uqudo.azurecr.io/node-app:latest .'
		docker.withRegistry('https://uqudo.azurecr.io', 'uqudo-acr') {
		sh 'docker push uqudo.azurecr.io/node-app:latest'
            } 
	   }
        }
	stage('Run Helm') {
	  container('helm') {
	    stage('Install node-app helm chart') {
            	sh 'helm upgrade --install node-app helm/node-app -n default'
	   }
	}
        }
    }
}
