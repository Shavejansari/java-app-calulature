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
                    sh 'pwd'
                    sh 'cd /var/lib/jenkins/workspace/javaAppPipeline/target'
                }
            }
        }
        stage('Artifact Upload') {
            steps {
                script {
                    sh 'curl -umohd@arintech.in:cmVmdGtuOjAxOjE3Mjc0Mzk3ODM6ZHVzUVdCclRqaFh2dXlFejJMVnpQNE5uRWFh -T Calculator-1.0-SNAPSHOT.jar "https://mohd.jfrog.io/artifactory/artifactory-build-info/”'
                }
            }
        }
        stage('Email notification') {
            steps {
                script {
                    emailext(
                        to: 'shavejkhan673@gmail.com',
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
