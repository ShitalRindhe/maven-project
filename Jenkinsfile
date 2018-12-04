pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to Staging'){
            steps {
				timeout(time:5, unit:'DAYS'){
					input message: 'Approve for the staging deployment?'
				}
                build job: 'Deploy-to-container'
            }
			post {
                success {
                    echo 'Code deployed to container.'
					emailext (subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'", body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p><p>Check console output at "<a href="${env.BUILD_URL}">${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>"</p>""", to: 'shitalr@kpit.com')
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }

    }
}