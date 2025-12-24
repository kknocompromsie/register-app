pipeline {
    agent { label 'Jenkins-Agent' }
    
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps { // Fixed: 'steps' must be lowercase
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/kknocompromsie/register-app'
            }
        }

        stage("Build Application") { // Fixed: Added missing opening double quote
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("SonarQube Analysis") {
    steps {
        script {
            withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                // Using the full artifact ID prevents the "No plugin found" error
                sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar"
            }
        }
    }
}
        stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
            }

        }
    }
}
