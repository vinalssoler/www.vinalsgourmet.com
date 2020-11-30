```groovy
pipeline {
    agent {
	label 'vg'
    }
    stages {
	stage ('build') {
	    steps{
		sh "hugo -D -F -b http://vg.vinals.local"
	    }
	}
	stage ('deploy') {
	    steps{
		sh 'rsync -r "$WORKSPACE/public/" git@172.26.0.14:/opt/docker/hugo/vg/public/'
	    }
	}

    }
}
```