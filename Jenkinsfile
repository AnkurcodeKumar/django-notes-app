library("Shared")

pipeline {
    agent { label 'vinod' }

    stages {

        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage('Clone Code') {
            steps {
                script{
                   clone('https://github.com/AnkurcodeKumar/django-notes-app.git', 'main')
                }
            }
        }

        stage('Build') {
            steps {
               script{
                   docker_build("notes-app","latest","ankur4556")
               }
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'This is pushing the image to DockerHub'

                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCred",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]) {

                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    sh "docker tag notes-app:latest ${dockerHubUser}/notes-app:latest"
                    sh "docker push ${dockerHubUser}/notes-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                     --remove-orphans
                    docker compose up -d
                '''
            }
        }
    }
}
