pipeline {
    agent any

    environment {
        // Specify the Node.js version to use; adjust if necessary
        NODE_VERSION = '16' // Replace with the version you need
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from GitHub
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], // Adjust branch name if needed
                    userRemoteConfigs: [[
                        url: 'https://github.com/your-username/your-repo.git', // Replace with your GitHub repository URL
                        credentialsId: 'your-github-credentials-id' // Replace with the credentials ID you created
                    ]]
                ])
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    // Install the specified Node.js version
                    def nodeHome = tool name: "NodeJS ${env.NODE_VERSION}", type: 'NodeJS'
                    env.PATH = "${nodeHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                // Build the Angular application
                sh 'ng build --prod'
            }
        }

        stage('Run Unit Tests') {
            steps {
                // Run unit tests
                sh 'ng test --watch=false --browsers=ChromeHeadless'
            }
        }

        stage('Run End-to-End Tests') {
            steps {
                // Run end-to-end tests
                sh 'ng e2e --headless'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
            // Archive artifacts and test results
            archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
            junit '**/TEST-*.xml'
        }
        success {
            echo 'Build and tests succeeded.'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
