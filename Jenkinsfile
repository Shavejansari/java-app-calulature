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

        stage('Code Quality Check') {
            steps {
                script {
                    env.SONAR_TOKEN = '3c415559dbef1aeed2f4b00202f8c7e3c2d0ac58'
                    sh 'env'
                    echo "SONAR_TOKEN: ${env.SONAR_TOKEN}"
                    sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=shavej-devops'
                }
            }
        }

        stage('Notify Stakeholders') {
            steps {
                script {
                    emailext(
                        subject: "Pipeline Status: ${currentBuild.result}",
                        body: """<p>Your Jenkins pipeline has completed with the following status: ${currentBuild.result}</p>""",
                        to: 'mohd@arintech.in', // Replace with the email address of the recipient
                        cc: 'shavejmohd30@gmail.com', // Optional: CC addresses
                        bcc: '', // Optional: BCC addresses
                        replyTo: '', // Optional: Reply-To address
                        mimeType: 'text/html' // Set to 'text/plain' for plain text emails
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
