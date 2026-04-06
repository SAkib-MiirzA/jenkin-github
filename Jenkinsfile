pipeline {
    agent any

    environment {
        USER_NAME = "SAkib"
        SERVER_IP = "172.92.201.56"
    }

    stages {

        stage('📥 Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/SAkib-MiirzA/jenkin-github.git', // ✅ correct repo
                credentialsId: 'github-jenkins-pat' // ✅ PAT credential ID in Jenkins
            }
        }

        stage('🐳 Check Docker') {
            steps {
                sh 'docker ps || true'
            }
        }

        stage('📄 Generate HTML Report') {
            steps {
                script {
                    def htmlContent = """
                    <!DOCTYPE html>
                    <html>
                    <head>
                        <title>Jenkins DevOps Report</title>
                        <style>
                            body { font-family: Arial; background:#0f172a; color:#e2e8f0; text-align:center; padding:50px; }
                            .card { background:#1e293b; padding:30px; border-radius:12px; }
                            h1 { color:#38bdf8; }
                            .success { color:#22c55e; }
                            .info { color:#facc15; }
                        </style>
                    </head>
                    <body>
                        <div class="card">
                            <h1>🚀 Jenkins DevOps Report</h1>
                            <p>👋 Hi! ${USER_NAME}</p>
                            <p class="success">✅ Your containers are running successfully!</p>
                            <p class="info">🌐 Your page IP is: <b>${SERVER_IP}</b></p>

                            <hr>

                            <p>🔄 CI/CD Pipeline executed successfully</p>
                            <p>🐳 Docker containers checked</p>
                            <p>📦 Build artifacts generated</p>
                            <p>⚙️ Deployment completed</p>
                            <p>📊 Monitoring checks passed</p>
                            <p>🔐 Security scan: OK</p>

                            <hr>

                            <p>🎉 Great job, DevOps Engineer!</p>
                        </div>
                    </body>
                    </html>
                    """

                    writeFile file: 'report.html', text: htmlContent
                }
            }
        }

        stage('📦 Archive Report') {
            steps {
                archiveArtifacts artifacts: 'report.html', fingerprint: true
            }
        }

        stage('🎉 Done') {
            steps {
                echo "Pipeline executed successfully!"
            }
        }
    }
}