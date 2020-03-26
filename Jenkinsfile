pipeline{
    agent any
    stages{
        stage("One"){
            steps{
                echo "========executing One========"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========A executed successfully========"
                }
                failure{
                    echo "========A execution failed========"
                }
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
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}