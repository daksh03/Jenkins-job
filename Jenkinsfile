pipeline {
    agent {
        label 'Slave-Node'  // Replace with the actual label of your slave node
    }
    
    tools {
        maven 'Maven 3.x'  // Ensure this matches the Maven installation name in Jenkins
    }

    
    environment {
         SONARQUBE_SERVER = 'MySonar'  // Replace with the name of your SonarQube server in Jenkins
    }
    

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/daksh03/Jenkins-job.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package' // Use 'bat' instead of 'sh' if running on Windows
            }
        }

        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    withCredentials([string(credentialsId: 'aws-sonar', variable: 'SONAR_TOKEN')]) {
                        // Run SonarQube analysis using Maven with token from Jenkins credentials
                        sh "mvn sonar:sonar -Dsonar.projectKey=d1 -Dsonar.host.url=http://44.202.58.37:9000 -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        
    }
}
