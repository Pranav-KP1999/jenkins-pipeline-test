pipeline {
    // This 'agent none' at the top level means stages can define their own agents.
    // This is a 'distributed' build, where different stages run on different agents.
    agent none

    stages {
        stage('Build and Test (on Master)') {
            agent {
                // It's good practice to run initial build/linting stages on the master or a dedicated build agent.
                // For simplicity, we'll use the 'Built-in Node' (Jenkins Master) here.
                label 'built-in' // This refers to your Jenkins master itself.
            }
            steps {
                script {
                    echo "--- Stage: Build and Test ---"
                    echo "Checking out Git repository from develop branch..."
                    // Checkout the develop branch. The job is configured for 'develop' too.
                    git branch: 'develop', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test.git' // REPLACE with your repository URL
                    echo "Repository cloned successfully into Jenkins Master workspace."

                    // Placeholder for actual build/test steps
                    // Example: If it's a Node.js app: sh 'npm install && npm test'
                    // Example: If it's a Java app: sh 'mvn clean install'
                    echo "Performing dummy build/test steps..."
                    sh 'ls -la' // Just list files to confirm checkout
                    sh 'echo "Tests passed!" > test_results.txt' // Simulate test results
                    echo "Build and test completed on Jenkins Master."
                }
            }
        }

        stage('Deploy to Test Environment') {
            agent {
                // This stage will execute on the agent labeled 'test'
                label 'test' // Refers to your Jenkins-Test-Agent
            }
            steps {
                script {
                    echo "--- Stage: Deploy to Test Environment ---"
                    echo "Copying files to test deployment directory on test-server..."
                    def deployDir = '/opt/test-app' // Same as in Assignment 2, Part 2.b
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"
                    // Copy everything from the workspace of the 'Build and Test' stage (master's workspace)
                    // to the test agent's deployment directory.
                    // 'currentBuild.getWorkspace()' refers to the workspace on the 'built-in' node where checkout happened.
                    sh "sudo cp -R ${currentBuild.getWorkspace()}/* ${deployDir}/"
                    echo "Files copied to ${deployDir} on test-server."

                    // Verify content on the test server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/test_deployment.txt" // Assuming this file exists from Assignment 2
                }
            }
        }

        // This stage will only execute if the 'Deploy to Test Environment' stage is successful
        stage('Deploy to Production Environment') {
            agent {
                // This stage will execute on the agent labeled 'prod'
                label 'prod' // Refers to your Jenkins-Prod-Agent
            }
            when {
                // This 'when' condition ensures this stage runs only if the previous one succeeded.
                // This is a common pattern for gate-keeping deployments.
                expression {
                    return currentBuild.previousBuild?.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo "--- Stage: Deploy to Production Environment ---"
                    echo "Deploying to production server..."
                    def deployDir = '/opt/prod-app' // Same as in Assignment 2, Part 2.c
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}"
                    // Copy everything from the workspace of the 'Build and Test' stage (master's workspace)
                    // to the prod agent's deployment directory.
                    sh "sudo cp -R ${currentBuild.getWorkspace()}/* ${deployDir}/"
                    echo "Files copied to ${deployDir} on prod-server."

                    // Verify content on the prod server
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/prod_deployment.txt" // Assuming this file exists from Assignment 2
                }
            }
        }
    }
}
