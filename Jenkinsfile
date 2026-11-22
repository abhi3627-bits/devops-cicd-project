pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/abhi3627-bits/devops-cicd-project.git'
            }
        }

        stage('Create Python Environment') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    .venv/bin/python -m pip install --upgrade pip
                    .venv/bin/python -m pip install -r requirements-dev.txt
                '''
            }
        }

        stage('Run Tests and Coverage') {
            steps {
                sh '''
                    .venv/bin/python -m pytest --cov=app --cov-report=term-missing --cov-report=xml
                '''
            }
        }

        stage('SonarQube Cloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonarcloud-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        set -e

                        curl -L -o sonar-scanner.zip \
                          https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-6.2.0.4584-linux-x64.zip

                        unzip -oq sonar-scanner.zip

                        export PATH="$PWD/sonar-scanner-6.2.0.4584-linux-x64/bin:$PATH"

                        sonar-scanner
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                      -t 2025tm93032/devops-flask-app:${BUILD_NUMBER} \
                      -t 2025tm93032/devops-flask-app:latest .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        set -e

                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin

                        docker push 2025tm93032/devops-flask-app:${BUILD_NUMBER}
                        docker push 2025tm93032/devops-flask-app:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    export KUBECONFIG=/var/lib/jenkins/.kube/config

                    /usr/local/bin/kubectl set image deployment/devops-flask-app \
                      flask-app=2025tm93032/devops-flask-app:${BUILD_NUMBER}

                    /usr/local/bin/kubectl rollout status deployment/devops-flask-app --timeout=120s
                '''
            }
        }
    }
}
