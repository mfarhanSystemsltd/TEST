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
			sh('ls')
                }
            }
        }
	stage('build and push image') {
            steps {
                script {
			buildImage 'mfarhan1998/simpleapp:${BUILD_NUMBER}'
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
							configName: 'server3', 
							transfers: 
							[
								sshTransfer(
									cleanRemote: false, 
									excludes: '', 
									execCommand: 'kubectl set image deployment/simpleapp-deployment simpleapp-container=mfarhan1998/simpleapp:${BUILD_NUMBER} --record', 
									execTimeout: 120000, 
									flatten: false, 
									makeEmptyDirs: false, 
									noDefaultExcludes: false, 
									patternSeparator: '[, ]+', 
									remoteDirectory: '', 
									remoteDirectorySDF: false, 
									removePrefix: '', 
									sourceFiles: ''
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
