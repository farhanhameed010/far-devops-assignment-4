pipeline {
    agent any
    
    parameters {
        choice(
            name: 'NODE_VERSION',
            choices: ['18-alpine', '16-alpine', '14-alpine'],
            description: 'Select Node.js version'
        )
        choice(
            name: 'MAVEN_VERSION',
            choices: ['3-jdk-8-alpine', '3-jdk-11-alpine', '3-jdk-17-alpine'],
            description: 'Select Maven version'
        )
        string(
            name: 'USER_NAME',
            defaultValue: 'developer',
            description: 'Enter your name'
        )
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // For public repo, no credentials needed
                git branch: 'main',
                    url: 'https://github.com/farhanhameed010/far-devops-assignment-4.git'
            }
        }
        
        stage('Node.js Environment') {
            steps {
                script {
                    docker.image("node:${params.NODE_VERSION}").inside {
                        // Check Node version and output with username
                        sh 'node -v > nodeVersion.txt'
                        def nodeVersion = readFile('nodeVersion.txt').trim()
                        echo "Node.js ${nodeVersion} - Checked by ${params.USER_NAME}"
                    }
                }
            }
        }
        
        stage('Maven Environment') {
            steps {
                script {
                    docker.image("maven:${params.MAVEN_VERSION}").inside {
                        // Check Maven version and output with username
                        sh 'mvn -v > mavenVersion.txt'
                        def mavenVersion = readFile('mavenVersion.txt').trim()
                        echo "Maven Version Check - ${mavenVersion} - Checked by ${params.USER_NAME}"
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed! Please check the logs."
        }
        always {
            cleanWs()
        }
    }
}