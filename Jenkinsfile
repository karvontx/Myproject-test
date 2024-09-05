pipeline {
    agent any

    environment {
        // Имя архива
        ZIP_NAME = "archived_txt_files.zip"
        // Путь, куда сохраняем архив на Jenkins агенте
        ARCHIVE_DIR = "${WORKSPACE}/archive"
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
                    // Создаем директорию для архива, копируем .txt файлы и архивируем их
                    sh '''
                        mkdir -p ${ARCHIVE_DIR}
                        find . -name "*.txt" -exec cp {} ${ARCHIVE_DIR}/ \\;
                        zip -r ${ARCHIVE_DIR}/${ZIP_NAME} ${ARCHIVE_DIR}
                    '''
                }
            }
        }

        stage('Save Archive') {
            steps {
                // Сохранение архива как артефакт в Jenkins
                archiveArtifacts artifacts: "${ARCHIVE_DIR}/${ZIP_NAME}", fingerprint: true
            }
        }
    }

    post {
        always {
            // Очищаем временные файлы, если необходимо
            echo "The archive is stored at ${ARCHIVE_DIR}/${ZIP_NAME} on the Jenkins agent."
        }
    }
}
