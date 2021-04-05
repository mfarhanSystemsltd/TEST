#!/usr/bin/env groovy

library identifier: 'js-jenkins-shared-library@main', retriever: modernSCM(
        [$class: 'GitSCMSource',
         remote: 'https://github.com/farru1998/js-shared-library-main.git',
         credentialsId: 'github-credentials'
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
                    buildImage 'mfarhan1998/simpleapp:1.0'
                    // dockerLogin()
                    // dockerPush 'armughanahmed/shared-lib-app:sla-3.0'
                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    gv.deployApp() 
                         withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '3ecc0f63-a467-4748-b965-53a8d15a3000', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
			       sh ('git add .')
			       sh ('git commit -m "heroku commit')
			       sh ('heroku git:remote -a aqueous-bayou-58074')
			       sh ('git push --force heroku master')
                         }
                }
            }
        }
    }
}
    
