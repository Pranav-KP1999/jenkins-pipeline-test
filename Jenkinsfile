pipeline {
    // This pipeline will run specifically on the agent labeled 'test' (your test-server)
    agent {
        label 'test'
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                script {
                    echo "Checking out Git repository from test branch..."
                    // This step automatically clones the repository configured in the job.
                    // Since the job is set to build 'test' branch, it will clone 'test'.
                    git branch: 'test', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test.git' // REPLACE with your repository URL
                    echo "Repository cloned successfully into agent's workspace."
                }
            }
        }
        stage('Deploy to Test Server') {
            steps {
                script {
                    echo "Copying files to test deployment directory..."
                    // Define the target deployment directory on the test server
                    def deployDir = '/opt/test-app' // You can change this path as needed

                    // Create the deployment directory if it doesn't exist on the agent
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}" // Grant ownership to ubuntu user

                    // Copy all contents from the Jenkins workspace to the deployment directory
                    // The workspace contains the cloned Git repository
                    sh "sudo cp -R * ${deployDir}/"
                    echo "Files copied to ${deployDir} on test-server."

                    // Verify content on the test server (optional)
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/test_deployment.txt" // Verify the test file
                }
            }
        }
    }
}
