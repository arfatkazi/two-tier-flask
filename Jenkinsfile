pipeline{
    agent { label "dev"};
    stages{
        stage("Code Clone"){
            steps{
                git url : "https://github.com/arfatkazi/two-tier-flask.git" , branch : "main"
                echo "Git hub clone completed"
            }
        }
        
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app:latest . "
                echo "Build was completed!"
            }
        }
        
        stage("Test"){
            steps{
                echo "Test  was completed!"
            }
        }
        
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId : "dockerHubCreds",
                    passwordVariable : "dockerHubPass",
                    usernameVariable : "dockerHubUser"
                )]){
                sh '''
                      echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                      docker tag two-tier-flask-app:latest $dockerHubUser/two-tier-flask-app:latest
                      docker push $dockerHubUser/two-tier-flask-app:latest
                    '''
                
                }
            }
        }
        
        
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
                echo "Deploy was successfully"
            }
        }
    }
    
    
    
    
    
}

