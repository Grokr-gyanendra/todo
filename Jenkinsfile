pipeline{
    agent any

    environment{
        IMAGE_NAME: 'gyanendranathshukla4035/test-app'
    }

    stages{
        stage('Build'){
            steps{
                script{
                    docker.build(${IMAGE_NAME})
                }
            }
        }
        
        stage('Test'){
            steps{
                script{
                    def path = pwd().replaceAll('C:', '/c').replaceAll('\\\\', '/')
                    sh "docker run -v ${path}:${path} -w ${path} ${IMAGE_NAME} pytest test_app.py"
                }
            }
        }
        stage('Push to Docker hub){
              steps {
                  script{
                      sh "docker login -u gyanendranathshukla4035 -p Prince2004"
                      sh "docker push ${IMAGE_NAME}"
                  }
              }
        }
            
        stage('Deploy'){
            steps{
                timeout(time: 2, unit: 'MINUTES') {
                    script{
                        def path = pwd().replaceAll('C:', '/c').replaceAll('\\\\', '/')
                        sh "docker run -d -v ${path}:${path} -w ${path} ${IMAGE_NAME} python app.py"
                    }
                }
            }
        }
    }

    post{
        success{
            echo "Depoyed Scccessfully !"
        }

        failure{
            echo "Failed Deplyment"
        }
    }
}
