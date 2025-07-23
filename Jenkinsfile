pipeline {
    agent any
    
    enviroment {
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
                                echo "📄 Before Update:"
                                cat deployment.yaml

                                sed -i 's+nnhainam/python-flask.*+nnhainam/python-flask:${params.DOCKERTAG}+g' deployment.yaml

                                echo "✅ After Update:"
                                cat deployment.yaml

                                git add .
                                git commit -m 'Done by Jenkins Job changemanifest: ${BUILD_NUMBER}'
                                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/demo-manifest.git HEAD:main
                            """
                        }
                    }
                }
            }

        }
    }
}