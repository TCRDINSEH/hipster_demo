pipeline {
    agent any

    environment {
        PROJECT_ID = 'fundamental-run-464208-v1'
        CLUSTER_NAME = 'tf-gke-cluster'
        CLUSTER_ZONE = 'us-central1-a'
        REGION = 'us-central1'
        GCP_CREDENTIALS = 'artifact-registry-credentials'
    }

    stages {

        stage('Checkout Code') {
            steps {
                sh '''
                    rm -rf front-end
                    git clone https://github.com/TCRDINSEH/front-end.git
                    cd front-end
                    pwd
                    ls -la
                '''
            }
        }

        /*stage('Setup Go Environment') {
            steps {
                dir('front-end') {
                    sh '''
                        echo "Setting up Go environment..."
                        go version || sudo apt-get update && sudo apt-get install -y golang-go
                        go mod tidy
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                dir('front-end') {
                    sh '''
                        echo "Building Go binary..."
                        go build -v -o app
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                dir('front-end') {
                    sh '''
                        echo "Running Go tests..."
                        go test ./... -v
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_HOST_URL = 'https://sonarcloud.io'
                SONAR_AUTH_TOKEN = credentials('sonarqube')
            }
            steps {
                dir('front-end') {
                    sh '''
                        echo "Running SonarQube analysis for Go..."
                        sonar-scanner \
                          -Dsonar.projectKey=TCRDINSEH_front-end \
                          -Dsonar.organization=TCRDIN \
                          -Dsonar.host.url=${SONAR_HOST_URL} \
                          -Dsonar.login=${SONAR_AUTH_TOKEN}
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('front-end') {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

        stage('Authenticate & Push to Artifact Registry') {
            steps {
                withCredentials([file(credentialsId: "${GCP_CREDENTIALS}", variable: 'GCLOUD_KEY')]) {
                    sh '''
                        echo "Authenticating with GCP..."
                        gcloud auth activate-service-account --key-file=$GCLOUD_KEY
                        gcloud auth configure-docker ${REGION}-docker.pkg.dev -q

                        echo "Pushing Docker image..."
                        docker tag ${IMAGE_NAME}:latest ${ARTIFACT_REGISTRY}/${IMAGE_NAME}:latest
                        docker push ${ARTIFACT_REGISTRY}/${IMAGE_NAME}:latest
                    '''
                }
            }
        }*/

        stage('Deploy to GKE') {
            steps {
                withCredentials([file(credentialsId: "${GCP_CREDENTIALS}", variable: 'GCLOUD_KEY')]) {
                    sh '''
                        echo "Deploying to GKE..."