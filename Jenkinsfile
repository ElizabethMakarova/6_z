pipeline {
    agent any
    parameters {
        choice(
            choices: ['dev', 'prod'],
            description: 'Выберите окружение для деплоя',
            name: 'DEPLOY_ENV'
        )
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Prepare') {
            steps {
                sh 'ls -la > filelist.txt'
                archiveArtifacts 'filelist.txt'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def remotePath = "/opt/app/${params.DEPLOY_ENV}"
                    
                    sshPublisher(
                        publishers: [
                            sshPublisherDesc(
                                configName: "internet",
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: "**/*",
                                        remoteDirectory: remotePath,
                                        execCommand: """
                                            mkdir -p ${remotePath}
                                            chmod -R 755 ${remotePath}
                                            echo 'Деплой успешен в ${remotePath}!'
                                            ls -la ${remotePath}
                                        """
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}