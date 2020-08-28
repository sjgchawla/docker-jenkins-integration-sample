pipeline{
    agent{
        label 'master'
    }

    environment{
        registry = "sjgchawla/jenkinsfile-test"
        registryCredential = "DOCKER_HUB"
        dockerImage = ""
    }

    parameters{
        choice(choices: 'a\nb\nc', description: 'Choose any one', name: 'ALPHA')
    }

    stages{
        stage('BUILDING IMAGE'){
            steps{
                script{
                    dockerImage = docker.build(registry + ":$BUILD_NUMBER")
                }
            }
        }

        stage('DEPLOY IMAGE'){
            steps{
                script{
                    docker.withRegistry('', registryCredential){
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Remove Unused docker image') {
            steps{
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}