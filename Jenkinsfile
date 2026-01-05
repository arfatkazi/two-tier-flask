
pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/arfatkazi/two-tier-flask.git", "main")
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
                echo "Developer / Tester tests likh ke dega..."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    withCredentials([usernamePassword(
                        credentialsId: "DockerHubCredentials",
                        passwordVariables: "dockerHubPass",
                        usernameVariable: "dockerHubUser" 

                                                      
                    )]{
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass} 
                        sh "docker image  tag two-tier-flask-app  ${env.dockerHubUser}/two-tier-flask-app"
                        sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    }
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }


}
