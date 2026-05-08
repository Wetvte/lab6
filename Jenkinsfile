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
                                            sourceFiles: "**/*"
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
                                            sourceFiles: "**/*"
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
