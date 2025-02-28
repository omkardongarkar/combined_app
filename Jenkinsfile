pipeline {
    agent any

    environment {
        MODULE1_REPO_URL = 'https://github.com/omkardongarkar/module1.git'
        MODULE2_REPO_URL = 'https://github.com/omkardongarkar/module2.git'
        COMBINED_REPO_URL = 'https://github.com/omkardongarkar/combined_app.git'
        COMBINED_REPO_DIR = 'combined-repo'
    }

    stages {
        stage('Clone Combined Repository') {
            steps {
                script {
                    // Clone the combined repository
                    git url: "${COMBINED_REPO_URL}", branch: 'master', credentialsId: 'github-credentials'
                }
            }
        }

        stage('Clone Module 1') {
            steps {
                script {
                    // Clone the module 1 repository
                    dir('module1') {
                        git url: "${MODULE1_REPO_URL}", branch: 'master', credentialsId: 'github-credentials'
                    }
                }
            }
        }

        stage('Clone Module 2') {
            steps {
                script {
                    // Clone the module 2 repository
                    dir('module2') {
                        git url: "${MODULE2_REPO_URL}", branch: 'master', credentialsId: 'github-credentials'
                    }
                }
            }
        }

        stage('Merge Repositories') {
            steps {
                script {
                    // Copy files from Module 1 to the combined repo
                    sh 'cp -r module1/* .'
                    sh 'rm -rf module1/.git'
                    // Copy files from Module 2 to the combined repo
                    sh 'cp -r module2/* .'
                    sh 'rm -rf module2/.git'
                }
            }
        }

        stage('Commit Changes') {
            steps {
                script {
                    sh '''
                       git config --global user.name "omkardongarkar"
                       git config --global user.email "domkar2690@gmail.com"
                    '''
                    // Add and commit changes
                    sh '''
                        git add .
                        git commit -m "Merged changes from Module 1 and Module 2" || echo "No changes to commit"
                    '''
                }
            }
        }

        stage('Push Changes') {
            steps {
                script {
                    // Push changes to the combined repository
                    sh '''
                         git push https://github.com/omkardongarkar/combined_app.git master --force
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up cloned repositories
            cleanWs()
        }
    }
}
