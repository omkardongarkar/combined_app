pipeline {
    agent any
    environment {
        COMBINED_REPO = 'https://github.com/omkardongarkar/combined_app.git'  // Replace with the URL of your combined_app repo
        MODULE1_REPO = 'https://github.com/omkardongarkar/module1.git'        // Replace with the URL of module1 repo
        MODULE2_REPO = 'https://github.com/omkardongarkar/module2.git'        // Replace with the URL of module2 repo
    }
    
    stages {
        stage('Clone Repositories') {
            steps {
                // Checkout the combined_app repo (the destination repo)
                checkout scm

                // Clone module1 and module2
                script {
                    sh 'git clone ${MODULE1_REPO} module1'
                    sh 'git clone ${MODULE2_REPO} module2'
                }
            }
        }

        stage('Combine Code') {
            steps {
                script {
                    // Example logic for combining the code from both modules
                    // You can move files, merge directories, etc.
                    // Here, we're just copying files into the combined_app repo

                    // Copy module1 files to the combined repo
                    sh 'cp -r module1/* .'

                    // Copy module2 files to the combined repo
                    sh 'cp -r module2/* .'
                    
                    // Example: You may want to organize them into separate directories
                    // sh 'cp -r module1/* combined_app/module1/'
                    // sh 'cp -r module2/* combined_app/module2/'

                    // Add combined changes
                    sh 'git add .'
                    sh 'git commit -m "Combine code from module1 and module2"'
                }
            }
        }

        stage('Push to combined_app Repository') {
            steps {
                script {
                    // Set user info for commit
                    sh 'git config user.email "domkar2690@gmail.com"'
                    sh 'git config user.name "omkardongarkar"'

                    // Push changes to the combined_app repo
                    sh 'git push origin main'  // Adjust to your branch name if different
                }
            }
        }
    }
    
    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
    }
}

