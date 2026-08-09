pipeline{
    
    agent { label "dev"}
    
    stages{
        
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
        stage('Push to Docker Hub') {
            steps {
        // Retrieve credentials configured in Jenkins (replace 'docker-hub-credentials' with your Credential ID)
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker tag two-tier-flask-app:latest $DOCKER_USER/two-tier-flask-app:latest
                docker push $DOCKER_USER/two-tier-flask-app:latest
            '''
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
