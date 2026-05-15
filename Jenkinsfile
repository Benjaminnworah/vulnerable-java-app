pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Benjaminnworah/vulnerable-java-app.git'
            }
        }

        stage('Dependency Scan') {
            steps {

                dependencyCheck additionalArguments: '--scan . --format XML',
                odcInstallation: 'dependency-check'
            }
        }
    }

    post {
        always {
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
        }
    }
}
