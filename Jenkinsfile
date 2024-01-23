pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('99c9a97d-9cb8-40f5-929b-217fa7cf1e0f')
        DOCKER_IMAGE_NAME = 'new_image'
        MAIN_BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/EngMohamedElEmam/new-app.git'
                }
            }
        }
    

        stage('Build and Dockerize') {
            when {
                // Run this stage only for the specified branch
                expression { env.BRANCH_NAME == "refs/heads/$MAIN_BRANCH" }
            }
            steps {
                // Build your application (adjust this based on your build tool, e.g., Maven, Gradle)
                sh 'mvn clean install'

                // Build a Docker image
                sh 'docker build -t new_image .'
                
            }
        }


        stage('Push to Docker Hub') {
            when {
                // Run this stage only for the main branch
                expression { env.BRANCH_NAME == "refs/heads/$MAIN_BRANCH" }
            }
            steps {
                // Push the Docker image to Docker Hub
                withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'monjj', passwordVariable: 'Amany*2023')]) {
                    sh "docker login -u monjj -p Amany*2023"
                    sh "docker tag new_image:latest monjj/java_test:latest "
                    sh "docker push monjj/java_test:latest "
                }
            }
        }
         stage('Pull from Docker Hub') {
            when {
                // Run this stage only for the main branch
                expression { env.BRANCH_NAME == "refs/heads/$MAIN_BRANCH" }
            }
            steps {
                // Pull the Docker image from Docker Hub (for demonstration purposes)
                sh "docker pull new_image"
            }
        }
    }


    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}