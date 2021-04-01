#!/usr/bin/env groovy

library identifier: 'js-jenkins-shared-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/armughanahmed/js-shared-library.git',
         credentialsId: 'github'
        ]
)

def gv

pipeline {
    agent any
    stages {
        stage('init') {
            steps {
                script {
                    gv = load 'script.groovy'
                }
            }
        }
        stage('check branch') {
            steps {
                script {
                    branch()
                }
            }
        }
        stage('build and push image') {
            steps {
                script {
                    buildImage 'armughanahmed/simpleapp:1.0'
                    // dockerLogin()
                    // dockerPush 'armughanahmed/shared-lib-app:sla-3.0'
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }
}
