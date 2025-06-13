pipeline {
    agent any
    
    tools {
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vanshrastogi111/Ekart.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage('Sonarqube analysis') {
    steps {
        withSonarQubeEnv('sonar-server') {
            sh """
                \$SCANNER_HOME/bin/sonar-scanner \\
                -Dsonar.projectName=Ekart \\
                -Dsonar.java.binaries=. \\
                -Dsonar.projectKey=Ekart
            """
        }
    }
}

        
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan target/', odcInstallation: 'owasp'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                   withDockerRegistry(credentialsId: '34932390-b59c-45d3-b257-54c349106d3a', toolName: 'docker') {
                        sh "docker build -t ekart -f docker/Dockerfile ."
                        sh "docker tag ekart vanshrastogi111/ekart:latest"
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '34932390-b59c-45d3-b257-54c349106d3a', toolName: 'docker') {
                        sh "docker push vanshrastogi111/ekart:latest"
                    }
                }
            }
        }
        
        stage('Deploy To Docker Container') {
            steps {
                script {
                    withDockerRegistry(credentialsId: '34932390-b59c-45d3-b257-54c349106d3a', toolName: 'docker') {
                        sh "docker run -d --name ekart -p 8083:8083 vanshrastogi111/ekart:latest"
                    }
                }
            }
        }
    }
}
