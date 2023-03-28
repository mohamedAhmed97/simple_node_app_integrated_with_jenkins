pipeline{
    agent any

    stages{
        stage("preparation"){
            steps{
                script {
                def packageJson = readJSON file: 'nodeapp/package.json'
                def packageVersion = packageJson.version
                def currentVersion="${packageVersion}-${BUILD_NUMBER}"
                echo "${currentVersion}"
            }
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
