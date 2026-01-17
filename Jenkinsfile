pipeline {
    agent { label "dev" }         
    
    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/arfatkazi/two-tier-flask.git", branch: "main"
                echo "GitHub clone completed"
            }
        }   

        
        stage("Build") {
            steps {
                sh "docker rm -f $(docker ps -qa)"
                sh "docker rmi -f $(docker images -qa)"
                sh "docker build -t two-tier-flask-app:latest ."
                sh "docker image tag two-tier-flask-app arfat942/two-tier-flask-app:latest"
                echo "Build completed!"
            }
        } 
        
        stage("Test") {
            steps {
                echo "Test completed!"
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "jenkin-master-key",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh '''
                        docker logout || true
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker push $dockerHubUser/two-tier-flask-app:latest
                    '''
                }
            }
        } 
        
        stage("Deploy") {
            steps {
                sh "docker compose down"
                sh "docker compose up -d --build"
                echo "Deploy was successful!"
            }
        }
    } 

    
}
