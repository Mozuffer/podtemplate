podTemplate(containers: [
	containerTemplate(name: 'docker', image: 'docker:19.03.5', ttyEnabled: true, command: 'cat'),
	containerTemplate(name: 'helm', image: 'alpine/helm:3.0.2', ttyEnabled: true, command: 'cat')],
	volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {
    node('mypod') {
        stage('build') {
            git 'https://github.com/Mozuffer/podtemplate.git'
	    container('docker'){
		sh 'docker build -t mozuffer/node-app:latest .'
	   }
        }
	stage('Run Helm') {
	  container('helm') {
	    stage('List all helm chart') {
            	sh 'helm ls --all-namespaces'
	   }
	}
        }
    }
}
