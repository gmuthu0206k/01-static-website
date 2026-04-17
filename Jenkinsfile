pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t static-site .'
            }
        }

        stage('Run') {
            steps {
                sh '''
                    docker rm -f static-site || true
                    docker run -d -p 8081:80 --name static-site static-site
                '''
            }
        }
    }
}
