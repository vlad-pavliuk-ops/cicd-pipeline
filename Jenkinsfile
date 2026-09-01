pipeline {

    agent any


    tools {
        nodejs 'node7'
    }


    environment {
        IMAGE_NAME = ''
        PORT = ''
    }


    stages {


        stage('Checkout') {

            steps {

                checkout scm

            }
        }


        stage('Build') {

            steps {

                sh 'npm install'

            }
        }


        stage('Test') {

            steps {

                sh 'npm test -- --watchAll=false'

            }
        }


        stage('Docker build') {

            steps {

                script {

                    if (env.BRANCH_NAME == 'main') {

                        IMAGE_NAME = 'nodemain:v1.0'
                        PORT = '3000'

                    } else if (env.BRANCH_NAME == 'dev') {

                        IMAGE_NAME = 'nodedev:v1.0'
                        PORT = '3001'

                    }


                    sh """
                    docker build -t ${IMAGE_NAME} .
                    """

                }
            }
        }



        stage('Deploy') {

            steps {

                script {


                    sh """

                    docker stop ${BRANCH_NAME}-app || true

                    docker rm ${BRANCH_NAME}-app || true


                    docker run -d \
                    --name ${BRANCH_NAME}-app \
                    --expose ${PORT} \
                    -p ${PORT}:3000 \
                    ${IMAGE_NAME}

                    """

                }

            }
        }


    }
}