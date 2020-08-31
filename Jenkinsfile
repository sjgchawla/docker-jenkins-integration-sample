#!/usr/bin/env groovy
@Library("jenkins.pipeline.library") _

pipeline{
    agent{
        label 'master'
    }

    environment{
        registry = "sjgchawla/jenkinsfile-test"
        registryCredential = "DOCKER_HUB"
        dockerImage = ""
    }

    tools{
        maven 'M2_HOME'
    }

    parameters{
        choice(choices: 'a\nb\nc', description: 'Choose any one', name: 'ALPHA')
    }


    stages{
        stage('BUILD MAVEN'){
            steps{
                script {
                    glMavenBuild mavenGoals: "clean install docker-jenkins-integration-sample"
                }
            }
        }

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

    post{
        success{
            echo 'This runs when success'
            emailext body: "BUILD URL: ${BUILD_URL}",
                    subject: "$JOB_NAME",
                    to: 'gaurav_chawla6@optum.com'
        }
    }
}