pipeline {
    agent {
        label 'Jenkins-Agent'
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages {
        stage("Clean Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/yogi-17/register-apps'
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

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQubeScanner'
                    withSonarQubeEnv(installationName: 'SonarQubeServer', credentialsId: 'jenkins-sonarqube-enterprise-token') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}
