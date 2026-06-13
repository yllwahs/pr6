pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Выберите окружение для деплоя')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Репозиторий успешно склонирован"
            }
        }

        stage('Prepare Deploy') {
            steps {
                script {
                    echo "Deploying to ${params.ENV} environment"
                }
            }
        }

        stage('Deploy via SSH') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: 'Staging-Server',
                        transfers: [
                            sshTransfer(
                                sourceFiles: 'app.py, requirements.txt',
                                remoteDirectory: '/home/proger/app',
                                execCommand: 'echo "Files uploaded successfully for ${params.ENV}"'
                            )
                        ]
                    )
                ])
                echo "Файлы успешно скопированы на сервер"
            }
        }
    }

    post {
        success {
            echo "Деплой на ${params.ENV} прошел успешно!"
        }
        failure {
            echo "Ошибка деплоя на ${params.ENV}. Проверьте логи."
        }
    }
}
