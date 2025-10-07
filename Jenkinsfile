@Library('jenkins-shared-library') _

pipeline {
    agent any
    tools {
        maven 'Maven_3'
    }
    environment {
        DOCKER_IMAGE = 'trivialflea/demo-app:jma-3.0'
        DOCKER_USER = credentials('docker-username')
        DOCKER_PASS = credentials('docker-password')
    }
    stages {
        stage("Initialize") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("Build JAR") {
            steps {
                script {
                    try {
                        gv.buildJar()
                    } catch (Exception e) {
                        echo "Error building JAR: ${e.message}"
                        throw e
                    }
                }
            }
        }
        stage("Build Docker Image") {
            steps {
                script {
                    try {
                        gv.buildImage(env.DOCKER_IMAGE)
                    } catch (Exception e) {
                        echo "Error building Docker image: ${e.message}"
                        throw e
                    }
                }
            }
        }
        stage("Deploy to Production") {
            steps {
                script {
                    try {
                        gv.deployApp()
                    } catch (Exception e) {
                        echo "Error deploying application: ${e.message}"
                        throw e
                    }
                }
            }
        }
    }
}
