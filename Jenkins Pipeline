pipeline {
    agent any

    environment {
        // Переменная для хранения имени zip файла
        ZIP_NAME = "archived_txt_files.zip"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                // Клонируем репозиторий с GitHub
                git branch: 'main', url: 'https://github.com/karvontx/Myproject-test.git'
            }
        }

        stage('Archive .txt files') {
            steps {
                script {
                    echo 'Archiving .txt files...'
                    // Ищем все файлы с расширением .txt и архивируем их
                    sh '''
                        mkdir -p archive
                        find . -name "*.txt" -exec cp {} archive/ \\;
                        zip -r ${ZIP_NAME} archive/
                    '''
                }
            }
        }

        stage('Save Archive') {
            steps {
                // Сохранение архива как артефакт
                archiveArtifacts artifacts: ZIP_NAME, fingerprint: true
            }
        }
    }

    post {
        always {
            // Удаляем временные файлы и директории
            cleanWs()
        }
    }
}
