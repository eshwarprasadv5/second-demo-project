pipeline{
    agent{
        label 'MVN1'
    }
    tools{
        maven 'maven385'
    }
    options{
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
        timestamps()
        timeout(10)
    }
    stages{
        stage('CheckOut'){
            steps{
               git branch: 'branch1', url: 'https://github.com/eshwarprasadv5/second-demo-project.git'
            }
        }
        stage('Build'){
            steps{
                sh "mvn package"
            }
        }
        stage('Deploy'){
            parallel{
                stage('Deploy1'){
                    environment{
                        USER_NAME="ec2-user"                    
                        SERVER_IP="172.31.21.192"
                    }
                    steps{
                        sshagent(['maven-cd-key']) {
                        sh "scp -o StrictHostKeyChecking=no target/second-demo-project-2.0-SNAPSHOT.jar $USER_NAME@$SERVER_IP:/home/ec2-user" 
                         }
                    }
                }    
                stage('Deploy2'){
                    steps{
                        sh "echo Deploy 2 is processed"
                    }
                }
            }
        }
    }
        
    post{
            always{
                deleteDir()
            }
            failure{
                echo "Job failed"
            }
            success{
                echo "Job ran successfully"
            }
    }
        
    }
    
