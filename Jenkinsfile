pipeline {
    agent any
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Окружение для деплоя (dev или prod)')
    }
    stages {
        stage('Deployment Info') {
            steps {
                echo "Deploying to ${params.ENV}"
            }
        }
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Deploy to Server') {
            steps {
                script {
                    if (params.ENV == 'prod') {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: "Prod",
                                    transfers: [
                                        sshTransfer(
                                            sourceFiles: "**/*",
                                            removePrefix: "",
                                            remoteDirectory: "/app/prod/",
                                            execCommand: "systemctl restart app-prod"
                                            )
                                    ],
                                    usePromotionTimestamp: false,
                                    useWorkspaceInPromotion: false,
                                    verbose: false
                                )           
                            ]
                        )
                    } else if (params.ENV == 'dev') {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: "Dev",
                                    transfers: [
                                        sshTransfer(
                                            sourceFiles: "**/*",
                                            removePrefix: "",
                                            remoteDirectory: "/app/dev/",
                                            execCommand: "systemctl restart app-dev"
                                        )
                                    ],
                                    usePromotionTimestamp: false,
                                    useWorkspaceInPromotion: false,
                                    verbose: false
                                )
                            ]
                        )
                    }
                }
            }
        }
    }
    post {
        success {
            echo "Deployment to ${params.ENV} completed successfully!"
        }
        failure {
            echo "Deployment to ${params.ENV} failed!"
        }
    }
}
