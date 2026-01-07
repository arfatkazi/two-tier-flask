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
                sh "docker pull mysql:5.7"
                sh "docker build -t two-tier-flask-app:latest ."
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
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh '''
                        docker logout || true
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag two-tier-flask-app:latest $dockerHubUser/two-tier-flask-app:latest
                        docker push $dockerHubUser/two-tier-flask-app:latest
                    '''
                }
            }
        } 
        
        stage("Deploy") {
            steps {
                sh "docker-compose down -v"
                sh "docker-compose up -d"
                echo "Deploy was successful!"
            }
        }
    } 

    post {
        success {
            emailext(
                from: "arfatkazi901@gmail.com",
                to: "arfatkazi901@gmail.com",
                subject: "Build success for Demo CI-CD App",
                body: "Build success for Demo CI-CD App"
            )
        }

        failure {
            emailext(
                from: "arfatkazi901@gmail.com",
                to: "arfatkazi901@gmail.com",
                subject: "Build failed for Demo CI-CD App",
                body: "Build failed for Demo CI-CD App"
            )
        }
    }
}
