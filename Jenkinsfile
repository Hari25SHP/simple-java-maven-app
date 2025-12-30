pipeline {
    agent any

    tools {
        jdk 'Java'
        maven 'M2_HOME'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                git 'https://github.com/Hari25SHP/simple-java-maven-app.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t hari8382/simple-website:latest .'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push 8382/simple-website:latest'
            }
        }

        stage('Deploy to Tomcat (8082)') {
            steps {
                sh '''
                docker rm -f simple-website || true
                docker run -d \
                  --name simple-website \
                  -p 8082:8080 \
                 hari8382/simple-website:latest
                '''
            }
        }
    }
}
