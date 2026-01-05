
pipeline {
    agent { label "dev" }

    stages {
        stage("Code Clone") {
            steps {
                script {
                    git url : "https://github.com/arfatkazi/two-tier-flask.git", branch : "main"
                }
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh ke dega..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "DockerHubCredentials",
                        passwordVariables: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                    )]) {
                        // Log in to Docker Hub
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        
                        // Tag the image
                        sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:${env.BUILD_ID}"
                        
                        // Push the image to Docker Hub
                        sh "docker push ${env.dockerHubUser}/two-tier-flask-app:${env.BUILD_ID}"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker-compose up -d --build flask-app"
            }
        }
    }
}

