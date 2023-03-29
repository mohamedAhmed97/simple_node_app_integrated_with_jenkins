pipeline{
    agent any

    stages{
        stage("preparation"){
            steps{
                script {
                def packageJson = readJSON file: 'nodeapp/package.json'
                def packageVersion = packageJson.version.substring(0, packageJson.version.length() - 2)
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
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                        sh "docker push mar97/simple_node_app:$currentVersion"
                    }

                }   
            }
        }
         stage("pushing to repo"){
            steps{
                script{
                        dir('nodeapp'){
                            sh "npm version $currentVersion"
                        }
                        sshagent (credentials: ['github-cred']) {
                            sh 'git config --global user.email "user@jenkins.com"'
                            sh 'git config --global user.name "Jenkins"'
                            sh "git add ."
                            sh 'git commit -m "ci: version bumb "'
                            sh 'git remote set-url origin git@github.com:mohamedAhmed97/simple_node_app_integrated_with_jenkins.git'
                            sh 'git push origin master'
                        }
                    }
            }
        }
    }
    post{
        always{
            echo "========always======="
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
