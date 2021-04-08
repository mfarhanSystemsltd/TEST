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
        
        stage('deploy') {
            steps {
                script {
                    gv.deployApp() 
			sshPublisher(
					publishers: 
					     [
						     sshPublisherDesc
						     (
							configName: 'server2', 
							transfers: 
							[
								sshTransfer(
									cleanRemote: false, 
									excludes: '', 
									execCommand: 'kubectl set image deployment/simpleapp-deployment simpleapp-deployment=mfarhan1998/simpleapp:1.0 --record', 
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
