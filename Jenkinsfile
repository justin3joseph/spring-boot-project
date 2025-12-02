pipeline {
    agent { label 'docker-node' }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/justin3joseph/spring-boot-project.git',
                    credentialsId: 'github-creds'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh '''
                    docker build -t devops-demo:latest .
                    '''
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    // Stop container if already running
                    sh '''
                    docker rm -f devops-demo-container || true
                    docker run -d -p 9090:8080 --name devops-demo-container devops-demo:latest
                    '''
                }
            }
        }
    }
}
