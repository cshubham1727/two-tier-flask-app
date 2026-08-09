pipeline{
    
    agent { label "dev"}
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/cshubham1727/two-tier-flask-app.git", "master")
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
                    docker_push("dockerHubCreds","two-tier-flask-app")
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
