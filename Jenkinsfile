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
                git branch: 'main',
                    url: 'https://github.com/farhanhameed010/far-devops-assignment-4.git'
            }
        }
        
        stage('Node.js Environment') {
            steps {
                sh """
                    # Pull Node.js image
                    docker pull node:${params.NODE_VERSION}
                    
                    # Run Node.js container and get version
                    docker run --rm node:${params.NODE_VERSION} node -v > nodeVersion.txt
                    
                    # Display version with username
                    echo "Node.js \$(cat nodeVersion.txt) - Checked by ${params.USER_NAME}"
                """
            }
        }
        
        stage('Maven Environment') {
            steps {
                sh """
                    # Pull Maven image
                    docker pull maven:${params.MAVEN_VERSION}
                    
                    # Run Maven container and get version
                    docker run --rm maven:${params.MAVEN_VERSION} mvn -v | head -n 1 > mavenVersion.txt
                    
                    # Display version with username
                    echo "Maven \$(cat mavenVersion.txt) - Checked by ${params.USER_NAME}"
                """
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