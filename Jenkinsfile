pipeline {
    agent any

    environment {
        ZIP_NAME = "archived_txt_files.zip"
        ARCHIVE_DIR = "archive"
        // Укажите абсолютный путь, где вы хотите хранить архив на Jenkins агенте
        SAVE_PATH = "/var/lib/jenkins/archives"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/karvontx/Myproject-test.git'
            }
        }

        stage('Archive .txt files') {
            steps {
                script {
                    echo 'Archiving .txt files...'
                    sh '''
                        mkdir -p ${ARCHIVE_DIR}
                        find . -name "*.txt" -exec cp {} ${ARCHIVE_DIR}/ \\;
                        zip -r ${ARCHIVE_DIR}/${ZIP_NAME} ${ARCHIVE_DIR}
                    '''
                }
            }
        }

        stage('Save to Jenkins Agent') {
            steps {
                script {
                    // Копируем архив в постоянную директорию на агенте
                    sh '''
                        mkdir -p ${SAVE_PATH}
                        cp ${ARCHIVE_DIR}/${ZIP_NAME} ${SAVE_PATH}/
                    '''
                    echo "Archive saved at ${SAVE_PATH}/${ZIP_NAME}"
                }
            }
        }

        stage('Save Archive to Jenkins Web Interface') {
            steps {
                // Сохраняем архив как артефакт в Jenkins
                archiveArtifacts artifacts: "${ARCHIVE_DIR}/${ZIP_NAME}", fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
            echo "Pipeline completed. Archive stored at ${SAVE_PATH}/${ZIP_NAME} on Jenkins agent."
        }
    }
}
