pipeline {
    agent any
    environment {
            TELEGRAM_BOT_TOKEN = credentials('telegram-token') // Telegram bot
            TELEGRAM_CHAT_ID = credentials('telegram-chat-id') // Telegram bot chat id
    }
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }
        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh '''
                echo "Hello"
                '''
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            script {
                sh """
                      curl -s -X POST https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage \
                      -H 'Content-Type: application/json' \
                      -d '{
                            "chat_id": "${TELEGRAM_CHAT_ID}",
                            "text": "✅ Build ${currentBuild.result}\\nJob: ${currentBuild.fullDisplayName}\\nURL: ${env.BUILD_URL}",
                            "disable_notification": false
                          }'
                    """
            }
        }
        failure {
            script {
                sh """
                      curl -s -X POST https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage \
                      -H 'Content-Type: application/json' \
                      -d '{
                            "chat_id": "${TELEGRAM_CHAT_ID}",
                            "text": "❌❌❌ Build ${currentBuild.result}\\nJob: ${currentBuild.fullDisplayName}\\nURL: ${env.BUILD_URL}",
                            "disable_notification": false
                          }'
                    """
            }
        }
    }
}
