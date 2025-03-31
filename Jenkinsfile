pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'your-docker-image'
        DOCKER_REGISTRY = 'your-docker-registry'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://gitlab.com/your-repo.git'
            }
        }
        stage(' Analyze with Sonarqube') {
            steps {
                def sqUrl = "your_SONAR_URL"
                sh 'sonar-scanner \
                -Dsonar.projectKey=your-project \
                -Dsonar.host.url=http://your-sonarqube-server
                // .....'
            }
        }
        stage('Build Backend') {
            steps {
                sh 'cd backend && mvn clean package'
            }
        }
        stage('Build Frontend') {
            steps {
                sh 'cd frontend && npm install && npm run build'
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh '''
                    docker build -t $DOCKER_IMAGE:latest .
                    docker tag $DOCKER_IMAGE:latest $DOCKER_REGISTRY/$DOCKER_IMAGE:latest
                    docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest
                '''
            }
        }
        stage('Security Scan') {
            steps {
                sh 'trivy image $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                sh 'zap-cli quick-scan http://your-application-url'
            }
        }
        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook -i inventory.ini deploy.yml'
            }
        }
        stage('Notify alert') {
            steps {
                sh 'curl -X POST -H "Content-Type: application/json" -d "{\"message\": \"Build and Deployment Completed\"}" http://your-telegram-bot-url'
            }
        }
    }
}