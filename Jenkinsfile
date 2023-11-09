pipeline {
	agent any
	environment {
		DOCKER_HOST="unix://\$(pwd)/docker.sock"
		STAGE_INSTANCE="ubuntu@ec2-16-171-64-39.eu-north-1.compute.amazonaws.com"
	}
	stages {
		stage('Setup SSH tunnel') {
			steps {
				script {
					sh "ssh -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
					// sometimes it's not enough time to make a tunnel, add sleep
					sleep 5
				}
			}
		}
		stage('Deploy') {
			steps {
				script {
					sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
				}
			}
		}
	}
	post {
		always {
			script {
				sh "pkill -F /tmp/tunnel.pid"
			}
		}
	}
}
