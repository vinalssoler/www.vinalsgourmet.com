pipeline {
    agent any
    
    stages {
	stage ('build') {
	    steps{		
		gitlog()
		sh "hugo -D -F -b http://vg.vinals.local"		
	    }
		post {
			success {
				slackSend channel: '#vinalsgourmet-com', color: '#00FF00', message: ":smile: - Building website: PROJECT: ${env.JOB_NAME} - Usuari: ${GIT_USER} - Build Number: ${env.BUILD_NUMBER}"
            }
			failure {
				slackSend channel: '#vinalsgourmet-com', color: '#FF0000', message: ":frowning: - Building website: PROJECT: ${env.JOB_NAME} - Usuari: ${GIT_USER} - Build Number: ${env.BUILD_NUMBER}"
            }
        }
	}
	stage ('deploy') {
	    steps{
		gitlog()
		sh 'rsync -r "$WORKSPACE/public/" git@172.26.0.14:/opt/docker/hugo/vg/public/'
		}
		post {
			success {
				slackSend channel: '#vinalsgourmet-com', color: '#00FF00', message: ":thumbsup: - Deployment: PROJECT: ${env.JOB_NAME} - Usuari: ${GIT_USER} - Build Number: ${env.BUILD_NUMBER} - ${GIT_COMMIT}"
            }
			failure {
				slackSend channel: '#vinalsgourmet-com', color: '#FF0000', message: ":thumbsdown: - Deployment: PROJECT: ${env.JOB_NAME} - Usuari: ${GIT_USER} - Build Number: ${env.BUILD_NUMBER} - ${GIT_COMMIT}"
            }
        }   		   
	}

    }
}

def gitlog() {
    sh('git log -1 --pretty=%B > commit-log.txt')
    GIT_COMMIT=readFile('commit-log.txt')
	sh('git log --format="%ae" | head -1 > commit-author.txt')
	GIT_USER=readFile('commit-author.txt').trim()    
}