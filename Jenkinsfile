pipeline {
    agent any
    tools {
        maven 'MAVEN'
    }
    stages{
        
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/sujay-sonar']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sujay-shukla/jenkins-sonar-demo.git']])
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                echo 'Scanning Maven project'
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: 'sonar-token') { 
                        sh 'mvn sonar:sonar -Dsonar.projectKey=sonardemo -Dsonar.organization=sonar-test-sept -Dsonar.host.url=https://sonarcloud.io'
                        sh 'sleep 50'
                        
                    }
                }    
            }
        }
        stage('Test'){
            steps{
                echo 'Building Maven project'
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
                jacoco classPattern: '**/target/classes', exclusionPattern: '**/*Test*.class', execPattern: '**/target/jacoco.exec', inclusionPattern: '**/*.class', sourceExclusionPattern: 'generated/**/*.java', sourceInclusionPattern: '**/*.java'
            }
        }
    }
        

}
