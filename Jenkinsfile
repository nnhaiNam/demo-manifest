pipeline {
    agent any
    
    environment {
        IMAGE_NAME='nnhainam/python-flask'
    }

    parameters {
        string(name: 'DOCKERTAG', defaultValue: 'latest', description: 'Docker image tag')
    }

    stages {
        stage('Clone repository') {
            steps {
                checkout scm
            }
        }

        stage('Update GIT DEMO-MANIFEST') {
            steps {
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        withCredentials([usernamePassword(credentialsId: 'github',passwordVariable: 'GIT_PASSWORD',usernameVariable: 'GIT_USERNAME')]) {
                            sh """
                                
                                git config --global user.email "hainam000090@gmail.com"
                                git config --global user.name "nnhainam"

                                echo "📄 Before Update:"
                                cat deployment.yaml

                                sed -i 's+nnhainam/python-flask.*+nnhainam/python-flask:${params.DOCKERTAG}+g' deployment.yaml

                                echo "✅ After Update:"
                                cat deployment.yaml

                                git add .
                                git commit -m 'Done by Jenkins Job changemanifest: ${params.DOCKERTAG}'
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/demo-manifest.git HEAD:main
                            """
                        }
                    }
                }
            }

        }
    }
}