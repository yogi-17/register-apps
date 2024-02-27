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
                    // Retrieve SonarQube Scanner tool installation directory
                    def scannerHome = tool 'sonarqube-scanner'
                    // Run SonarQube Scanner
                    withSonarQubeEnv('sonarqube-server') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=test-sonarqube-practice \
                            -Dsonar.sources=. \
                            -Dsonar.css.node=. \
                            -Dsonar.host.url=https://sonarqubeenterprise.eng.zaxbys.com \
                            mvn sonar:sonar
                        """
                    }
                }
            }
        }
    }
}
