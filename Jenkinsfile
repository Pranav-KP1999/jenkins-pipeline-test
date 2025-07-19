pipeline {
    agent any

    stages {
        stage('Checkout Git Repository') {
            steps {
                script {
                    // This step automatically clones the repository configured in the job.
                    // Since we configured "Branches to build" to 'develop' in the Jenkins job,
                    // it will automatically check out the develop branch.
                    // The content will be pulled into the Jenkins workspace directory for this job.
                    echo "Cloning repository..."
                    git branch: 'develop', url: 'https://github.com/Pranav-KP1999/jenkins-pipeline-test' // REPLACE with your repository URL
                    echo "Repository cloned successfully!"
                    // List files to show content (optional, for verification)
                    sh 'ls -al'
                    sh 'pwd'
                    sh 'cat test.txt' // If you created test.txt in Step 5
                }
            }
        }
        stage('Verify Content') {
            steps {
                echo "Verifying content in the workspace..."
                // Add any other steps here to process the pulled content,
                // e.g., run tests, build artifacts, etc.
                echo "Content verification complete."
            }
        }
    }
}
