pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[url: 'https://github.com/Marwa-Lakrab/framework-cucumber-automation.git']]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('Run Tests & Generate Reports') {
            steps {
                script {
                    // Command to run tests and generate Allure & Cucumber reports
                    sh 'mvn test -Dsurefire.useFile=false -Dallure.results.directory=target/allure-results'
                   
                }
            }
        }

        stage('Generate Allure Report') {
            steps {
                script {
                    // Ensure the Allure CLI tool path is correct
                    sh '"C:\\Users\\marwa\\OneDrive\\Bureau\\Documents\\Proservices\\Formation automatisation des tests\\allure-2.32.0\\bin\\allure.bat" generate target/allure-results -c -o target/allure-report'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/allure-results/**, target/allure-report/**', allowEmptyArchive: false
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml' // Ensure test results are archived
            allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
        }
    }
}
