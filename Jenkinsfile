
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
                        usernameVariable: "dockerHubUser",
                        passwordVariable: "dockerHubPass"
                    )]) {
                        // Log in to Docker Hub securely using --password-stdin
                        sh """
                            echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin
                        """
                        
                        // Tag the image
                        sh "docker image tag two-tier-flask-app ${dockerHubUser}/two-tier-flask-app:${env.BUILD_ID}"
                        
                        // Push the image to Docker Hub
                        sh "docker push ${dockerHubUser}/two-tier-flask-app:${env.BUILD_ID}"
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

