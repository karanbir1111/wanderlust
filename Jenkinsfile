pipeline {
    agent any
    environment {
        SONAR_HOME = tool "SonarScanner"
    }
    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("SonarQubeServer") {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }

        stage("Sonar Quality Gate") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        /*
        stage("OWASP Dependency Check") {
            steps {
                withCredentials([string(credentialsId: 'nvd-api-key', variable: 'NVD_KEY')]) {
                    dependencyCheck additionalArguments: "--scan ./ --nvdApiKey $NVD_KEY --format HTML --format XML", odcInstallation: 'Owasp'
                }
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        */
        stage("Trivy File Scan") {
            steps {
                sh "trivy fs --format table --severity HIGH,CRITICAL --scanners vuln,secret,config ."
            }
        }

        stage("Build Docker Images") {
            steps {
                sh "docker compose build"
            }
        }

        stage("Deploy with Docker Compose") {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
}
