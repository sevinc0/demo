pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t demo-app:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f demo-app-container || true
                    docker run -d \
                        --name demo-app-container \
                        --network demo-network \
                        -p 8082:8080 \
                        demo-app:${BUILD_NUMBER}
                '''
            }
        }
    }
}