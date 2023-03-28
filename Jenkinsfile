pipeline{
    agent any

    stages{
        stage("preparation"){
            steps{
                script {
                def packageJson = readJSON file: 'nodeapp/package.json'
                def packageVersion = packageJson.version
                env.currentVersion="${packageVersion}.${BUILD_NUMBER}"
                echo "${currentVersion}"
            }
            }
        }

         stage("building"){
            steps{
                echo "========stage building ========"
                sh "docker build -t mar97/simple_node_app:$currentVersion ."
            }
        }

        stage("deployment"){
            steps{
                script{
                    echo "========stage deployment ========"
                    withCredentials([usernamePassword(credentialsId: 'github-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                        sh "docker push mar97/node_app:$currentVersion"
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
            // slackSend(color:#0fff,message:"hi")
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
