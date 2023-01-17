pipeline{
    agent any 
    stages{
        stage("github repo"){
            steps{
                git credentialsId: 'github-access', url: 'https://github.com/njokuifeanyigerald/CI-CD-with-jenkins-java-gradle-app.git'
            }
            post{
                success{
                    echo "========github repo executed successfully========"
                }
                failure{
                    echo "========github repo execution failed========"
                }
            }
        }
        stage("sonarQube Quality Gate analysis"){     
            // agent {
            //     docker {
            //         image 'openjdk:11'
            //     }
            // }
            
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
                success{
                    echo "====++++sonarQube analysis executed successfully++++===="
                }
                failure{
                    echo "====++++sonarQube analysis execution failed++++===="
                }
        
            }
        }
        stage("docker login to nexus "){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-nexus', variable: 'docker_password')]) {                        
                    //    sh 'docker build -t  localhost:8083/springapp:$BUILD_ID .'
                    //    sh 'docker login -u admin -p $docker_password localhost:8083'
                    //     sh 'docker push localhost:8083/springapp:$BUILD_ID'
                    //     sh 'docker rmi localhost:8083/springapp:$BUILD_ID'

                        sh '''
                            docker build -t  localhost:8083/springapp:$BUILD_ID .
                            docker login -u admin -p $docker_password localhost:8083
                            docker push localhost:8083/springapp:$BUILD_ID
                            docker rmi localhost:8083/springapp:$BUILD_ID
                        '''
                    

                    }
                    
                }
            }
            post{
                success{
                    echo "====++++docker login to nexus  executed successfully++++===="
                }
                failure{
                    echo "====++++docker login to nexus  execution failed++++===="
                }
        
            }
        }
    
    }
    post{
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}