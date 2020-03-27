pipeline{
    environment{
        IMAGE = "TestAppImg"
        VERSION = "001"
    }
    agent any
    stages{
        stage("One"){
            steps{
                echo "========executing One========"
            }
        }
        stage("Two"){
            steps{
                input('Do you want to proceed?')
            }
        }
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
        stage("Deploy"){
                    steps{
                            echo "Deploying Docker Container"
                    }
                }  
    }
}