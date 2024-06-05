pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                echo 'docker images build and run within a docker container!'
		sh 'docker ps -a'
		sh 'docker images'
            }
        }
    }
}
