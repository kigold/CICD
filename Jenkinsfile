pipeline{
    environment{
        IMAGE = "testing_app_img"
        VERSION = "001"
        APP = "test_app"
    }
    agent any
    stages{
        stage("One"){
            steps{
                echo "========executing One========"
            }
        }
        /*stage("Two"){
            steps{
                input('Do you want to proceed?')
            }
        }*/
        stage("Three"){
            when{
                not {
                        branch "master"
                }
            }
            steps{
                    echo "Hello"
            }
        }
        stage("Four"){
            parallel {
                stage("Unit Test"){
                    steps{
                            echo "Running the unit test..."
                    }
                }  
                stage("Integration Test"){
                    agent {
                        docker {
                            reuseNode true
                            image 'ubuntu'
                        }
                    }
                    steps{
                            echo "Running the integration test..."
                    }
                }                
            }
        }
        stage("Build"){
                    steps{
                            echo "Building Docker Image"
                            sh """
                                docker build -t ${IMAGE} .
                                docker tag ${IMAGE} ${IMAGE}:${VERSION}
                            """
                    }
                } 
        stage("Clean"){
                steps{
                        echo "Remove old Docker Container"
                        /*sh """
                        docker stop ${APP}
                        docker rm ${APP}
                        """*/
                        //make it fail safe
                        sh """
                        docker ps -f name=${APP} -q | xargs --no-run-if-empty docker stop
                        docker ps -a -f name=${APP} -q | xargs -r docker rm
                        """
                }
            }  
        stage("Deploy"){
                    steps{
                            echo "Deploying Docker Container"
                            sh """
                            docker run -p 8765:80 --name ${APP} ${IMAGE}
                            """
                    }
                }  
    }
}