pipeline{
    agent any 
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
        // stage("sonarQube Quality Gate analysis"){     
        //     // agent {
        //     //     docker {
        //     //         image 'openjdk:11'
        //     //     }
        //     // }
            
        //     steps{
        //         script{
        //             withSonarQubeEnv(credentialsId: 'sonarQubeToken') {
        //                 sh 'chmod +x gradlew'
        //                 sh './gradlew sonarqube'
        //             }
        //             timeout(time: 1, unit: 'HOURS') {
        //               def qg = waitForQualityGate()
        //               if (qg.status != 'OK') {
        //                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
        //               }
        //             }
        //         } 
        //     }
        //     post{
        //         success{
        //             echo "====++++sonarQube analysis executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++sonarQube analysis execution failed++++===="
        //         }
        
        //     }
        // }
        // stage("docker login to nexus "){
        //     steps{
        //         script{
        //             withCredentials([string(credentialsId: 'docker-nexus', variable: 'docker_password')]) {                        
        //             //    sh 'docker build -t  localhost:8083/springapp:$BUILD_ID .'
        //             //    sh 'docker login -u admin -p $docker_password localhost:8083'
        //             //     sh 'docker push localhost:8083/springapp:$BUILD_ID'
        //             //     sh 'docker rmi localhost:8083/springapp:$BUILD_ID'

        //                 sh '''
        //                     docker build -t  localhost:8083/springapp:$BUILD_ID .
        //                     docker login -u admin -p $docker_password localhost:8083
        //                     docker push localhost:8083/springapp:$BUILD_ID
        //                     docker rmi localhost:8083/springapp:$BUILD_ID
        //                 '''
                
        //             }
                    
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++docker login to nexus  executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++docker login to nexus  execution failed++++===="
        //         }
        
        //     }
        // }
        // stage("Datree"){
        //     steps{
        //         echo "====++++executing Datree++++===="
        //         script{
        //             withEnv(['DATREE_TOKEN=d35945da-d7af-421e-8397-c50c36aa3c69']) {
        //                 sh 'datree test *.yaml --only-k8s-files'
        //             }
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++Datree executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++Datree execution failed++++===="
        //         }
        
        //     }
        // }
        // stage("pushing the helm chart to nexus"){
        //     steps{
        //         echo "====++++executing pushing the helm chart to nexus++++===="
        //         script{
        //             withCredentials([string(credentialsId: 'docker-nexus', variable: 'docker_password')]) {                        
        //                 sh '''
        //                     helmversion=$( helm show chart ./kubernetes/myapp | grep version | cut -d: -f 2 | tr -d ' ')
        //                     tar  -czvf myapp-${helmversion}.tgz ./kubernetes/myapp
        //                     curl -u admin:$docker_password http://127.0.0.1:8081/repository/helm-gerald/ --upload-file myapp-${helmversion}.tgz -v
        //                 '''
                
        //             }
                    
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++pushing the helm chart to nexus executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++pushing the helm chart to nexus execution failed++++===="
        //         }
        
        //     }
        // }


        stage("deploying k8s"){
            steps{
                echo "====++++executing deploying k8s++++===="
                script {
                    kubeconfig(credentialsId: 'myKubeConfig', serverUrl: 'https://192.168.59.101:8443') {
                            // sh 'kubectl get nodes'
                            // sh 'kubectl apply -f kube.yaml'
                            // sh 'kubectl get all'

                            sh 'helm upgrade --install --set image.repository="127.0.0.1:8083/springapp" --set image.tag="${VERSION}" javaspringboot /kubernetes/myapp/ ' 
                    }
                                
                }
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++deploying k8s executed successfully++++===="
                }
                failure{
                    echo "====++++deploying k8s execution failed++++===="
                }
        
            }
        }

        
    
    }
    post{
        // always {
		// 	mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}",
        //     cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '',
        //     subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "brainboyrichmond@gmail.com";  
		// }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        } 
    }   
}