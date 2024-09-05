pipeline {
    agent any

    environment {
        ZIP_NAME = "archived_txt_files.zip"
        ARCHIVE_DIR = "archive"
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

        stage('Save Archive') {
            steps {
                echo "Saving archive..."
                // Используем относительный путь
                archiveArtifacts artifacts: "${ARCHIVE_DIR}/${ZIP_NAME}", fingerprint: true
            }
        }
    }

    post {
        always {
            cleanWs()
            echo "The archive is saved at ${WORKSPACE}/${ARCHIVE_DIR}/${ZIP_NAME}"
        }
    }
}
