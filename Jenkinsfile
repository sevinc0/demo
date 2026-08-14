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

        stage('Health Check') {
            steps {
                sh '''
                    echo "Uygulamanın başlaması bekleniyor..."

                    for i in {1..18}
                    do
                        echo "Health check denemesi: $i/18"

                        if curl -f http://localhost:8082/actuator/health; then
                            echo ""
                            echo "Uygulama başarıyla çalışıyor!"
                            exit 0
                        fi

                        sleep 5
                    done

                    echo "Uygulama sağlık kontrolünden geçemedi."
                    docker logs demo-app-container
                    exit 1
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline başarıyla tamamlandı!'
        }

        failure {
            echo 'Pipeline başarısız oldu.'
        }
    }
}