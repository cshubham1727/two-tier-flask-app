@Library("Shared") _

pipeline {

    agent { label "dev" }

    stages {

        stage("Code Clone") {
            steps {
                script {
                    clone("https://github.com/cshubham1727/two-tier-flask-app", "master")
                }
            }
        }

        stage("Trivy File System Scan") {
            steps {
                script {
                    trivy_scan()
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

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker_push("dockerHubCreds", "two-tier-flask-app")
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
