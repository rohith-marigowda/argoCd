pipeline {
agent any
  stages {
        stage('Checkout SCM'){
            steps {
		git 'https://github.com/rohith-marigowda/argoCd'
		sh "echo printing docker tag $DOCKERTAG"
            }
        }


	  stage('Updating Kubernetes deployment file'){
            steps {
		    script{
			sh "echo $JOB_NAME"
			sh "cat deployment.yaml"
			sh "sed -i 's#rohithmarigowda/assignment.*#rohithmarigowda/assignment:${DOCKERTAG}#g' deployment.yaml"
                	sh "cat deployment.yaml"
		    }
            }
        }
	  
stage('Update Deployment File') {
	   steps{
		script{
				catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
					withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                    sh """
                    git config --global user.name "Rohith"
                    git config --global user.email "rohith@gmail.com"
                    git add deployment.yaml
                    git commit -m 'Updated the deployment file' """
                	sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:master"        
					}
				}
			}
	   }
   }

	  
  }
}
