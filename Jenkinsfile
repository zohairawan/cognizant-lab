pipeline {
    agent none
        environment {
        ENV_DOCKER = credentials('docker')
        DOCKERIMAGE = "zohair89/cognizant-lab"
        EKS_CLUSTER_NAME = "sre-lab"
    }
    stages {
        stage('Clone Repo') {
            agent {
                docker { image 'openjdk:11-jdk' }
            }
            steps {
                script {
                    git branch: 'master', url: "https://github.com/zohairawan/cognizant-lab.git"
                    sh 'chmod +x gradlew && ./gradlew build jacocoTestReport'
                 }
            }
        }
        stage('sonarqube') {
        agent {
            docker { image '<some sonarcli image>' } }
            steps {
                sh 'echo scanning!'
            }
        }
        stage('Docker Image Build') {
            steps {
                script {
                    echo "Building docker image..."
                    dockerImage = docker.build("zohair89/cognizant-lab:${env.BUILD_NUMBER}")
                 }
             }
        stage('Docker Deploy'){
            steps {
                script {
                    echo "Uploading Docker image to Dockerhub"
                     //docker-hub-credentials - we have to create in jenkins credentials
                    docker.withRegistry('https://registry.hub.docker.com/','docker') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy App') {
            steps {
                sh 'echo deploy to kubernetes'               
            }
    }
}
}
