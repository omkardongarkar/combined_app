pipeline {
    agent any  // This means it can run on any available agent (node) in Jenkins

    environment {
        REPO1 = "https://github.com/omkardongarkar/module1.git" // URL of module1 repo
        REPO2 = "https://github.com/omkardongarkar/module2.git" // URL of module2 repo
        NEW_REPO = "https://github.com/omkardongarkar/combined_app.git" // URL of the new combined repo
        NEW_REPO_DIR = "combined_repo" // Directory to clone the new repo
    }

    stages {
        stage('Clone Repositories') {
            steps {
                script {
                    // Clone module1 and module2 repositories into the Jenkins workspace
                    def module1Clone = git url: REPO1, branch: 'master', changelog: false, poll: false
                    def module1Clone2 = git url: REPO2, branch: 'master', changelog: false, poll: false
                }
            }
        }

        stage('Create New Repository') {
            steps {
                script {
                    // Create a new directory for the new combined repository
                    sh "mkdir ${NEW_REPO_DIR}"
                    dir(NEW_REPO_DIR) {
                        // Initialize a new git repository in the new directory
                        sh 'git init'
                        sh "git remote add origin ${NEW_REPO}"
                    }
                }
            }
        }

        stage('Move Module1 and Module2 Files') {
            steps {
                script {
                    // Move files from module1 and module2 into separate directories inside the new repo
                    sh "mkdir ${NEW_REPO_DIR}/module1"
                    sh "mkdir ${NEW_REPO_DIR}/module2"
                    sh "cp -r module1/* ${NEW_REPO_DIR}/module1/"
                    sh "cp -r module2/* ${NEW_REPO_DIR}/module2/"
                }
            }
        }

        stage('Commit and Push Changes') {
            steps {
                script {
                    // Commit and push the changes to the new combined repository
                    dir(NEW_REPO_DIR) {
                        sh "git add ."
                        sh "git commit -m 'Combine module1 and module2 into the new repository'"
                        sh "git push -u origin main"
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Clean up by removing the temporary cloned directories
                    sh "rm -rf module1"
                    sh "rm -rf module2"
                }
            }
        }
    }

    post {
        success {
            echo "Repositories have been successfully combined and pushed!"
        }
        failure {
            echo "An error occurred during the process."
        }
    }
}

