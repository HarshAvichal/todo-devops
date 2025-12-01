pipeline {
    agent any
    
    tools {
        nodejs 'NodeJS-24'
    }
    
    environment {
        NODE_VERSION = '24'
        BUILD_DIR = 'dist'
        NOTIFICATION_EMAIL = 'harshavichal08@gmail.com'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '📥 Checking out code from repository...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo '📦 Installing npm dependencies...'
                sh 'npm ci'
            }
        }
        
        stage('Lint Code') {
            steps {
                echo '🔍 Running ESLint...'
                sh 'npm run lint'
            }
        }
        
        stage('Build Application') {
            steps {
                echo '🏗️  Building for production...'
                sh 'npm run build'
            }
        }
        
        stage('Test Build') {
            steps {
                echo '✅ Verifying build output...'
                sh '''
                    if [ -d "${BUILD_DIR}" ]; then
                        echo "✓ Build directory exists"
                        ls -lh ${BUILD_DIR}
                    else
                        echo "✗ Build directory not found!"
                        exit 1
                    fi
                '''
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                echo '📦 Archiving build artifacts...'
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*", 
                                 fingerprint: true,
                                 allowEmptyArchive: false
            }
        }
        
        stage('Deploy to Vercel Production') {
            steps {
                echo '🚀 Deploying to Vercel Production...'
                withCredentials([string(credentialsId: 'vercel-token', variable: 'VERCEL_TOKEN')]) {
                    sh 'npx vercel --prod --token=${VERCEL_TOKEN} --yes'
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline completed successfully!'
            // Uncomment to enable email notifications (requires SMTP configuration)
            // emailext (
            //     subject: "✅ Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //     body: "The build completed successfully!",
            //     to: "${NOTIFICATION_EMAIL}"
            // )
        }
        
        failure {
            echo '❌ Pipeline failed!'
            // emailext (
            //     subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //     body: "The build failed. Check Jenkins for details.",
            //     to: "${NOTIFICATION_EMAIL}"
            // )
        }
        
        always {
            echo '🧹 Pipeline execution completed'
        }
    }
}
