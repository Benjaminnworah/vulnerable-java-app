pipeline {

    agent any

    environment {
        NVD_API_KEY = credentials('nvd-api-key')
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Benjaminnworah/vulnerable-java-app.git'
            }
        }

        stage('Build Maven Dependencies') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Dependency Scan') {
            steps {

                dependencyCheck additionalArguments: """
                    --project vulnerable-java-app
                    --scan .
                    --scan /var/lib/jenkins/.m2
                    --format XML
                    --format HTML
                    --enableExperimental
                    --nvdApiKey ${NVD_API_KEY}
                    --disableKnownExploited
                """,
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
