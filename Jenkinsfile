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
                // ... (your existing installation steps)
            }
        }

        stage('Compiling and Running Test Cases') {
            steps {
                // ... (your existing compilation and testing steps)
            }
        }

        stage('Code Quality Check') {
            steps {
                // ... (your existing SonarQube analysis steps)
            }
        }

        stage('Artifact Upload') {
            steps {
                script {
                    def username = 'mohd@arintech.in'
                    def apiKey = 'cmVmdGtuOjAxOjE3Mjc0MjU0OTA6Ym96WExQMHROY05pTEZRalk1Y1RockVva2p2'
                    def sourceFilePath = 'Calculator-1.0-SNAPSHOT.jar'
                    def targetFilePath = 'default-generic-local/Calculator-1.0-SNAPSHOT.jar'
                    def artifactoryUrl = 'https://mohd.jfrog.io/artifactory/'

                    def curlCommand = """
                        curl -u ${username}:${apiKey} -T ${sourceFilePath} "${artifactoryUrl}${targetFilePath}"
                    """

                    if (isUnix()) {
                        sh curlCommand
                    } else {
                        bat curlCommand
                    }
                }
            }
        }

        stage('Notify Stakeholders') {
            steps {
                script {
                    emailext(
                        to: 'mohd@arintech.in',
                        subject: 'Pipeline Status',
                        body: '''Hi Mohd,
                        Greetings of the day
                        Your Jenkins pipeline has completed with the following status
                        Regards
                        Mohd Shavej''')
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
