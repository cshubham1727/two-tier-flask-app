pipeline {

    agent { label "dev" }

    stages {

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

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCreds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker tag two-tier-flask-app:latest $DOCKER_USER/two-tier-flask-app:latest
                        docker push $DOCKER_USER/two-tier-flask-app:latest
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Build Successful",
                body: "Good News: Your build was successful!",
                to: 'choudhary7763@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "Build Failed",
                body: "Bad News: Your build has Failed!",
                to: 'choudhary7763@gmail.com'
            )
        }
    }
}
