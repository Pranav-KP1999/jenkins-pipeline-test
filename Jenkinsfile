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
                    git branch: 'develop', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test.git' // REPLACE with your repository URL
                    echo "Repository cloned successfully into Jenkins Master workspace."

                    echo "Performing dummy build/test steps..."
                    sh 'ls -la'
                    sh 'echo "Tests passed!" > test_results.txt'
                    echo "Build and test completed on Jenkins Master."

                    // STASH the files so they can be accessed by other agents
                    // We'll stash everything in the workspace after checkout
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
                    // UNSTASH the files stashed from the 'Build and Test' stage
                    unstash 'source-code'
                    echo "Source code unstashed on test-server."

                    echo "Copying files to test deployment directory on test-server..."
                    def deployDir = '/opt/test-app'
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"

                    // Copy all contents from the current agent's workspace (where unstash put them)
                    sh "sudo cp -R * ${deployDir}/"
                    echo "Files copied to ${deployDir} on test-server."

                    // Verify content on the test server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/test_deployment.txt"
                }
            }
        }

        stage('Deploy to Production Environment') {
            agent {
                label 'prod' // Jenkins-Prod-Agent
            }
            when {
                expression {
                    return currentBuild.previousBuild?.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo "--- Stage: Deploy to Production Environment ---"
                    // UNSTASH the files again for the production deployment
                    unstash 'source-code'
                    echo "Source code unstashed on prod-server."

                    echo "Deploying to production server..."
                    def deployDir = '/opt/prod-app'
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"

                    // Copy all contents from the current agent's workspace (where unstash put them)
                    sh "sudo cp -R * ${deployDir}/"
                    echo "Files copied to ${deployDir} on prod-server."

                    // Verify content on the prod server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/prod_deployment.txt"
                }
            }
        }
    }
}
