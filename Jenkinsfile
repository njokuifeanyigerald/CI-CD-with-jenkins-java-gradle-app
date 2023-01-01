pipeline{
    // agent any
    agent {
        docker {
            image 'openjdk:11'
        }
    }
    stages{
        stage("github repo"){
            steps{
                git credentialsId: 'github-access', url: 'https://github.com/njokuifeanyigerald/CI-CD-with-jenkins-java-gradle-app.git'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========github repo executed successfully========"
                }
                failure{
                    echo "========github repo execution failed========"
                }
            }
        }
        stage("sonarQube Quality Gate analysis"){     
            
            
            steps{
                script{
                    

                    withSonarQubeEnv(credentialsId: 'sonarQubeToken') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                } 
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++sonarQube analysis executed successfully++++===="
                }
                failure{
                    echo "====++++sonarQube analysis execution failed++++===="
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