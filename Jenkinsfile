pipeline {
    agent none

    stages {
        stage('Build and Test (on Master)') {
            agent {
                label 'built-in' // Jenkins Master
            }
            steps {
                script {
                    echo "--- Stage: Build and Test ---"
                    echo "Checking out Git repository from develop branch..."
                    // IMPORTANT: Ensure this URL is correct and only has one 'https://'
                    git branch: 'develop', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test.git'
                    echo "Repository cloned successfully into Jenkins Master workspace."

                    echo "Performing dummy build/test steps..."
                    sh 'ls -la'
                    sh 'echo "Tests passed!" > test_results.txt'
                    echo "Build and test completed on Jenkins Master."

                    stash includes: '**/*', name: 'source-code'
                    echo "Source code stashed for deployment."
                }
            }
        }

        stage('Deploy to Test Environment') {
            agent {
                label 'test' // Jenkins-Test-Agent
            }
            steps {
                script {
                    echo "--- Stage: Deploy to Test Environment ---"
                    unstash 'source-code'
                    echo "Source code unstashed on test-server."

                    echo "Copying files to test deployment directory on test-server..."
                    def deployDir = '/opt/test-app'
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"
                    // This must copy ALL files from the current workspace after unstashing
                    sh "sudo cp -R * ${deployDir}/" // <-- ENSURE THIS IS '*'
                    echo "Files copied to ${deployDir} on test-server."

                    // Verify content on the test server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/test_deployment.txt"
                }
            }
        }

        // Removed the 'when' block entirely. If the previous stage succeeds, this one will run.
        // If the previous stage fails, the pipeline will stop.
        stage('Deploy to Production Environment') {
            agent {
                label 'prod' // Jenkins-Prod-Agent
            }
            steps {
                script {
                    echo "--- Stage: Deploy to Production Environment ---"
                    unstash 'source-code'
                    echo "Source code unstashed on prod-server."

                    echo "Deploying to production server..."
                    def deployDir = '/opt/prod-app'
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"
                    // This must copy ALL files from the current workspace after unstashing
                    sh "sudo cp -R * ${deployDir}/" // <-- ENSURE THIS IS '*'
                    echo "Files copied to ${deployDir} on prod-server."

                    // Verify content on the prod server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/prod_deployment.txt"
                }
            }
        }
    }
}
