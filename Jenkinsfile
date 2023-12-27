pipeline {
    agent {
        node { label "dev-agent" }
    }

    stages {
        stage('Code') {
            steps {
                // Clone the open-source Git repository
                git url: 'https://github.com/akshay-bl/jenkins-pipeline-exercise.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        // Use Gradle wrapper for consistency
                        sh './gradlew clean test jar --stacktrace --debug'
                    } catch (Exception e) {
                        // Handle errors gracefully
                        currentBuild.result = 'FAILURE'
                        error('Build failed with: ${e.getMessage()}')
                    }
                }
            }
        }

        stage('Result') {
            steps {
                // Publish JUnit test results
                junit '**/build/test-results/test/TEST-*.xml'

                // Archive artifacts - the JAR file
                archiveArtifacts 'build/libs/*'
            }
        }
    }

    post {
        always {
            // Monitor resource usage
            sh 'free -m'
            sh 'df -h'
        }
    }
}
