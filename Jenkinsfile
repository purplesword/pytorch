#!groovy
@Library('gini-shared-library@master') _


pipeline {
    agent {
        label 'base'
    }

    environment {
            PROJECT_NAME = getProjectName()
        }

    options {
        gitLabConnection('git.i.gini.net')
    }

    stages {

        stage('Build and publish docker image of python 3.6') {
            steps {
                withDockerRegistry([credentialsId: 'gini-registry', url: 'https://hub.i.gini.net/']) {
                    sh "docker build -t hub.i.gini.net/3a/pytorch_py36:${env.BUILD_NUMBER} -e PYTHON_VERSION=3.6 -f docker/pytorch/Dockerfile ."

                    sh "docker push hub.i.gini.net/3a/pytorch_py36:${env.BUILD_NUMBER}"
                }
            }
        }

        stage('Build and publish docker image of python 3.7') {
            steps {
                withDockerRegistry([credentialsId: 'gini-registry', url: 'https://hub.i.gini.net/']) {
                    sh "docker build -t hub.i.gini.net/3a/pytorch_py37:${env.BUILD_NUMBER} -e PYTHON_VERSION=3.7 -f docker/pytorch/Dockerfile ."

                    sh "docker push hub.i.gini.net/3a/pytorch_py37:${env.BUILD_NUMBER}"
                }
            }
        }
    }
    post {
        failure {
            updateGitlabCommitStatus name: 'pipeline', state: 'failed'
        }
        success {
            updateGitlabCommitStatus name: 'pipeline', state: 'success'
        }
    }
}