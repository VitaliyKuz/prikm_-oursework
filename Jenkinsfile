pipeline {
    agent any

    environment {
        API_TOKEN = credentials('API_TOKEN')
        WEATHER_API_KEY = credentials('WEATHER_API_KEY')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/VitaliyKuz/prikm-bot-cursach.git'
            }
        }

        stage('Build and Run Docker Container') {
            steps {
                script {
                    def image = docker.build("weather-bot-image")
                    
                    // Зупинити та видалити існуючий контейнер, якщо він існує
                    sh """
                        if [ \$(docker ps -aq -f name=weather-bot-container) ]; then
                            docker stop weather-bot-container
                            docker rm weather-bot-container
                        fi
                    """
                    
                    // Запустити новий контейнер
                    image.run('-d -p 5000:5000 -p 8000:8000 --name weather-bot-container -e API_TOKEN=${API_TOKEN} -e WEATHER_API_KEY=${WEATHER_API_KEY}')
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
