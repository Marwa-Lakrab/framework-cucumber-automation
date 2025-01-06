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
                    sh 'mvn test -Dcucumber.options="--plugin json:target/cucumber.json" -Dallure.results.directory=target/allure-results'
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
                // Archive results for both Allure and Cucumber
                archiveArtifacts artifacts: 'target/allure-results/**, target/allure-report/**, target/cucumber.json', allowEmptyArchive: false
            }
        }
    }

    post {
        always {
            // Archive JUnit test results
            junit 'target/surefire-reports/*.xml'
            
            // Publish Allure report
            allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]

            // Generate and publish Cucumber JSON report
            publishCucumberReports(
                fileIncludePattern: 'target/cucumber.json', 
                fileExcludePattern: '', 
                sortingMethod: 'ALPHABETICAL'
            )
        }
    }
}
