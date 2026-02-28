@Library("Shared") _
pipeline{
    
    agent { label "dev" }
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/denish10/two-tier-flask-app.git", "master")
               }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Running tests..."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerhubtoken", "two-tier-flask-app")
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose down || true"
                sh "docker compose up -d flask-app"
            }
        }
    }
}
