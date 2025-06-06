pipeline {
    agent any
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
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: "internet",
                            transfers: [
                                sshTransfer(
                                    sourceFiles: "**/*",
                                    remoteDirectory: "/opt/app/dev",
                                    execCommand: """
                                        mkdir -p /opt/app/dev
                                        chmod -R 755 /opt/app/dev
                                        echo 'Деплой успешен!'
                                        ls -la /opt/app/dev
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
