pipeline {
    agent any

    // Build parameters
    parameters {
        choice(name: 'REPO_NAME', choices: ['GitHub-CV', 'GitLab-Task'], description: 'Select which repo to build')
        choice(name: 'GIT_METHOD', choices: ['HTTPS', 'SSH'], description: 'Select Git access method')
    }

    environment {
        // GitHub
        GITHUB_HTTPS = "https://github.com/SAkib-MiirzA/jenkin-github.git"
        GITHUB_SSH   = "git@github.com:SAkib-MiirzA/jenkin-github.git"
        CRED_GITHUB_HTTPS = "github-https"  // Jenkins Credential ID for GitHub HTTPS
        CRED_GITHUB_SSH   = "github-ssh"    // Jenkins Credential ID for GitHub SSH

        // GitLab
        GITLAB_HTTPS = "https://gitlab.com/sakib00786/gitlab-task.git"
        GITLAB_SSH   = "git@gitlab.com:sakib00786/gitlab-task.git"
        CRED_GITLAB_HTTPS = "gitlab-https"  // Jenkins Credential ID for GitLab HTTPS
        CRED_GITLAB_SSH   = "gitlab-ssh"    // Jenkins Credential ID for GitLab SSH
    }

    stages {

        stage(' Checkout Repo') {
            steps {
                script {
                    def repoUrl = ""
                    def credId = ""

                    if (params.REPO_NAME == 'GitHub-CV') {
                        if (params.GIT_METHOD == 'HTTPS') {
                            repoUrl = env.GITHUB_HTTPS
                            credId  = env.CRED_GITHUB_HTTPS
                        } else {
                            repoUrl = env.GITHUB_SSH
                            credId  = env.CRED_GITHUB_SSH
                        }
                    } else { // GitLab-Task
                        if (params.GIT_METHOD == 'HTTPS') {
                            repoUrl = env.GITLAB_HTTPS
                            credId  = env.CRED_GITLAB_HTTPS
                        } else {
                            repoUrl = env.GITLAB_SSH
                            credId  = env.CRED_GITLAB_SSH
                        }
                    }

                    echo "Checking out repo: ${repoUrl}"
                    git branch: 'main', url: repoUrl, credentialsId: credId
                }
            }
        }

        stage(' Verify Files') {
            steps {
                sh '''
                echo "Listing files in workspace..."
                ls -la
                '''
            }
        }

        stage(' Show Info') {
            steps {
                script {
                    def ip = sh(script: "hostname -I | awk '{print \$1}'", returnStdout: true).trim()
                    echo "======================================"
                    echo "Repo: ${params.REPO_NAME}"
                    echo "Git Method: ${params.GIT_METHOD}"
                    echo "Jenkins Build URL: ${env.BUILD_URL}"
                    echo "Server IP: http://${ip}"
                    echo "======================================"

                    if (params.REPO_NAME == 'GitHub-CV') {
                        echo "GitHub Pages Preview: https://jenkin-github/"
                    }
                }
            }
        }

        stage(' Archive Repo') {
            steps {
                archiveArtifacts artifacts: '**/*', fingerprint: true
            }
        }

        stage(' Done') {
            steps {
                echo " Pipeline executed successfully!"
            }
        }
    }
}
