pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t tpachatproject:latest .'
            }
        }

        stage('Scan Vulnerabilities') {
            steps {
                echo 'Scanning for vulnerabilities...'
                sh 'trivy image tpachatproject:latest'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    echo 'Logging in to Docker Hub...'
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    echo 'Pushing Docker Image...'
                    sh 'docker tag tpachatproject:latest <your-dockerhub-username>/tpachatproject:latest'
                    sh 'docker push <your-dockerhub-username>/tpachatproject:latest'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
