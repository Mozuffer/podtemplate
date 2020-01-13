podTemplate(containers: [containerTemplate(name: 'helm', image: 'alpine/helm:3.0.2', ttyEnabled: true, command: 'cat')]) {
    node(POD_LABEL) {
        stage('Run shell') {
            sh 'echo hello world'
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
