pipeline {
	agent any

	environment {
		IMAGE_NAME = 'my-flask-app'
		CONTAINER_NAME= 'flask-app'
	}

	stages {

		stage('Checkout') {
			steps {
				echo 'pulling code from github....'
				checkout scm
			}
		}

		stage('Build Docker Image') {
			steps {
				echo 'Building docker image ..'
				sh 'docker build -t ${IMAGE_NAME} .'
			}
		}

		stage('Test') {
			steps {
				echo 'Running tests..'
				sh 'docker run --rm ${IMAGE_NAME} python -c "import flask;print(flask.__version__)"'
		}
	}

	stage('Deploy with Ansible')  {
		steps {
			echo 'Deploying with Ansible...'
			sh 'ansible-playbook deploy.yml'
		}
	}
}

	post {
		success {
			echo 'Pipeline completed successfully! App is live.'
		}
		failure {
			echo 'Pipeline failed.Check the logs above.'
		}
	}
}
