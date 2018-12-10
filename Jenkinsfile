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
        stage('Build docker image') {
            steps {
                withDockerRegistry([credentialsId: 'gini-registry', url: 'https://hub.i.gini.net/']) {
                    sh "docker build -t hub.i.gini.net/3a/pytorch_v1_0_0_cpu_jessie:${env.BUILD_NUMBER} --build-arg PYTHON_VERSION=3.6 -f docker/pytorch/Dockerfile.cpu.jessie ."
                }
            }
        }

        stage('Push docker image') {
            steps {
                withDockerRegistry([credentialsId: 'gini-registry', url: 'https://hub.i.gini.net/']) {
                    sh "docker push hub.i.gini.net/3a/pytorch_v1_0_0_jessie:${env.BUILD_NUMBER}"
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