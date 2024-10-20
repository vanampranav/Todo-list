pipeline {
    agent any

    environment {
        GITHUB_REPO = "https://github.com/vanampranav/Todo-list.git"
        RENDER_API_KEY = credentials('render-api-key')  // Render API key stored in Jenkins credentials
        SERVICE_ID = "https://api.render.com/deploy/srv-csakrfrqf0us739vphjg?key=XF6S7Z2eyvg"       // Your Render static site ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO}"
            }
        }

        stage('Build') {
            steps {
                // Run any build command if required for static site generators (e.g., npm run build for React)
                // If no build is needed, this can be skipped.
                sh 'npm install'  // Example: installing dependencies
                sh 'npm run build'  // Example: building a React/Next.js/Vue.js static site
            }
        }

        stage('Deploy to Render') {
            steps {
                script {
                    // Trigger a new deployment to Render using the Render API
                    sh """
                    curl -X POST https://api.render.com/v1/static/${SERVICE_ID}/deploys \\
                    -H "Authorization: Bearer ${RENDER_API_KEY}" \\
                    -H "Accept: application/json" \\
                    -H "Content-Type: application/json"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Static site deployed successfully!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
