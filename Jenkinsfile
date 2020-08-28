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
                    dockerImage = docker.build(registry + ":${params.ALPHA}")
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
    }
}