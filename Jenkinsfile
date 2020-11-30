pipeline {
    agent any
    
    stages {
	stage ('build') {
	    steps{
		sh "hugo -D -F -b http://vg.vinals.local"
	    }
	}
	stage ('deploy') {
	    steps{
		sh 'rsync -r "$WORKSPACE/public/" git@172.26.0.14:/opt/docker/hugo/vg/public/'
		slackSend (color: '#FFFF00', message: "Deployed: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
	    }
	}

    }
}