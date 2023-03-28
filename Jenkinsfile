pipeline{
    agent any

    stages{
        stage("preparation"){
            steps{
                echo "========stage preparation ========"
                echo "1.0.$BUILD_NUMBER"
            }
        }

         stage("building"){
            steps{
                echo "========stage building ========"
            }
        }

        stage("deployment"){
            steps{
                echo "========stage deployment ========"
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
            // slackSend(color:#0fff,message:"hi")
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
