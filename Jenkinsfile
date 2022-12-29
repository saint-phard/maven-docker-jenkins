#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/saint-phard/jenkins-shared-library.git',
     credentialsId: 'my-github-credentials'
    ]
)
def groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.8.6'
    }

    stages {
        stage('initialise groovy script') {
            steps {
                script {
                    echo 'initializing the groovy script'
                    groovy = load "script.groovy"
                }
            }
        }
        stage('build jar') {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    buildImage 'phard/the-app:maven-2.0'
                    dockerLogin()
                    dockerPush 'phard/the-app:maven-2.0'
                }
            }    
          }
        stage('deploy') {
            steps {
                script {
                    groovy.deployApp()
                }
            }
        }
    }
}
