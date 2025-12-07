pipeline {
    agent any
    environment {
        CI = 'true'
        PATH = "${env.PATH}:/usr/local/bin:/opt/homebrew/bin"
    }
    stages {
        stage('Setup Node.js') {
            steps {
                script {
                    // Check if node is available
                    def nodeCheck = sh(script: 'command -v node 2>/dev/null || echo "not found"', returnStdout: true).trim()
                    if (nodeCheck == 'not found') {
                        // Try to install Node.js using Homebrew (macOS)
                        sh '''
                            if command -v brew &> /dev/null; then
                                echo "Installing Node.js via Homebrew..."
                                brew install node
                            else
                                echo "ERROR: Node.js not found and Homebrew not available."
                                echo "Please install Node.js manually or configure a NodeJS tool in Jenkins."
                                exit 1
                            fi
                        '''
                    }
                    // Verify node and npm are available
                    sh 'node --version && npm --version'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
    }
}