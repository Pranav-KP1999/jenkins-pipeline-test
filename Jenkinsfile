pipeline {
    // This pipeline will run specifically on the agent labeled 'prod' (your prod-server)
    agent {
        label 'prod'
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                script {
                    echo "Checking out Git repository from master branch..."
                    // This step automatically clones the repository configured in the job.
                    // Since the job is set to build 'master' branch, it will clone 'master'.
                    git branch: 'master', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test.git' // REPLACE with your repository URL
                    echo "Repository cloned successfully into agent's workspace."
                }
            }
        }
        stage('Deploy to Prod Server') {
            steps {
                script {
                    echo "Copying files to production deployment directory..."
                    // Define the target deployment directory on the prod server
                    def deployDir = '/opt/prod-app' // You can change this path as needed

                    // Create the deployment directory if it doesn't exist on the agent
                    sh "sudo mkdir -p ${deployDir}"
                    sh "sudo chown -R ubuntu:ubuntu ${deployDir}" // Grant ownership to ubuntu user

                    // Copy all contents from the Jenkins workspace to the deployment directory
                    // The workspace contains the cloned Git repository
                    sh "sudo cp -R * ${deployDir}/"
                    echo "Files copied to ${deployDir} on prod-server."

                    // Verify content on the prod server (optional)
                    sh "ls -al ${deployDir}"
                    sh "cat ${deployDir}/prod_deployment.txt" // Verify the prod file
                }
            }
        }
    }
}
