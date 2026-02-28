pipeline {
    agent { label 'dev' }
    stages {
        stage("Code Clone") {
            steps {
                echo "Cloning repository..."
                git url: 'https://github.com/denish10/two-tier-flask-app.git', branch: 'master'
            }
        }
        stage("Build") {
            steps {
                echo "Building Docker image..."
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test") {
            steps { echo "Running tests..." }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubtoken",
                    usernameVariable: "dockerhubuser",
                    passwordVariable: "dockerhubpass"
                )]) {
                    sh "docker login -u ${dockerhubuser} -p ${dockerhubpass}"
                    sh "docker tag two-tier-flask-app ${dockerhubuser}/two-tier-flask-app:latest"
                    sh "docker push ${dockerhubuser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying using Docker Compose..."
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
