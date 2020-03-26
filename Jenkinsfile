pipeline{
    agent any
    stages{
        stage("One"){
            steps{
                echo "========executing One========"
            }
        }
    }
    stages{
        stage("Two"){
            steps{
                input('Do you want to proceed?')
            }
        }
    }
    stages{
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
    }
    stages{
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
    }
}