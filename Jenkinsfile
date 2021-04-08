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
			sshPublisher(
					publishers: 
					     [
						     sshPublisherDesc
						     (
							configName: 'server1', 
							transfers: 
							[
								sshTransfer(
									cleanRemote: false, 
									excludes: '', 
									execCommand: '', 
									execTimeout: 120000, 
									flatten: false, 
									makeEmptyDirs: false, 
									noDefaultExcludes: false, 
									patternSeparator: '[, ]+', 
									remoteDirectory: '//var/www/example.com/html', 
									remoteDirectorySDF: false, 
									removePrefix: '', 
									sourceFiles: '**'
								)
						], 
						usePromotionTimestamp: false, 
						useWorkspaceInPromotion: false, 
						verbose: false
						     )
					     ]
				    )
                }
            }
        }
    }
}
