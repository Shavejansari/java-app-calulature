pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
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
                        sh 'sudo apt-get update -y'
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
        stage('Code Quality Check SonarQube') {
            steps {
                script {
                    env.SONAR_TOKEN = '3c415559dbef1aeed2f4b00202f8c7e3c2d0ac58'
                    echo "SONAR_TOKEN: ${env.SONAR_TOKEN}"
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=shavej-devops'
                    sh 'ls'
                 }
            }
        }
        stage('Artifact Upload to JFrog') {
             steps {
                 script {
                    dir('/var/lib/jenkins/workspace/javaAppPipeline/target') {
                    sh 'ls'
                    sh 'curl -umohd@arintech.in:cmVmdGtuOjAxOjE3Mjc0OTY3MzQ6N0dBODFYTjYzYnpnM1A5Q1V2bFcwSWdLa2RU -T /var/lib/jenkins/workspace/javaAppPipeline/target/Calculator-3.1.0-SNAPSHOT.jar "https://mohd.jfrog.io/artifactory/default-generic-local/"'
                    }     
                 }
             }
        }
        stage('Email notification') {
            steps {
                script {
                    emailext(
                        to: 'thakuranoop54321@gmail.com',
                        subject: 'Pipeline Status',
                        body: '''Hi Mohd,
                        Greetings of the day
                        Your Jenkins pipeline has completed with the following status
                        Regards
                        Mohd Shavej'''
                    )
                }
            }
        }
    }
    post {
        success {
            script {
                currentBuild.result = 'SUCCESS'
            }
        }
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }
    }
}
