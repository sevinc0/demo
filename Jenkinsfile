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
                    i=1

                    while [ $i -le 12 ]
                    do
                        echo "Health check denemesi: $i"

                        if curl -f http://127.0.0.1:8082/actuator/health; then
                            echo "Uygulama hazır!"
                            exit 0
                        fi

                        echo "Uygulama henüz hazır değil, 5 saniye bekleniyor..."
                        sleep 5

                        i=$((i + 1))
                    done

                    echo "Uygulama sağlık kontrolünden geçemedi."
                    echo "Container logları:"
                    docker logs demo-app-container

                    exit 1
                '''
            }
        }
    }
}