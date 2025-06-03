pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        SONAR_TOKEN = credentials('jenkins-sonarqube-token')
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Soni-ak/register-app.git'
            }
        }

        stage("Build Application") {
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
                    // Use exact name of your SonarQube installation configured in Jenkins
                    withSonarQubeEnv('sonarqube-server') {
                        sh """
                            mvn sonar:sonar \
                            -Dsonar.projectKey=register-app \
                            -Dsonar.login=$SONAR_TOKEN
                        """
                    }
                }
            }
        }
    }
}
