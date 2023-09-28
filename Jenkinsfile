pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Installing Java and Maven') {
            steps {
                script {
                    def maven_exists = fileExists '/usr/share/maven'
                    if (maven_exists == true) {
                        echo "Skipping maven install - already exists"
                    } else {
                        sh ‘sudo apt-get  update -y’
                        sh 'sudo apt install -y wget tree unzip openjdk-11-jdk maven'
                    }
                }
            }
        }

        stage('Compiling and Running Test Cases') {
            steps {
                sh 'mvn clean'
                sh 'mvn compile'
                sh 'mvn test'
            }
        }
