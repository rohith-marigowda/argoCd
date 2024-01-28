pipeline {
agent any
  stages {
        stage('Checkout SCM'){
            steps {
		git 'https://github.com/rohith-marigowda/argoCd'
		// 1. Create a new feature branch
		sh "git branch -D feature/argoCdCiCd"
		sh "git checkout -b feature/argoCdCiCd"
            }
        }


	  stage('Updating Kubernetes deployment file'){
            steps {
		    script{
			sh "cat deployment.yaml"
			sh "sed -i 's#rohithmarigowda/assignment.*#rohithmarigowda/assignment:${DOCKERTAG}#g' deployment.yaml"
                	sh "cat deployment.yaml"
		    }
            }
        }
	  
stage('Update Deployment File') {
    steps {
        script {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                withCredentials([usernamePassword(credentialsId: 'githubcred', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {

                    // 2. Commit your changes to the feature branch
                    sh """
                    git config --global user.name "Rohith"
                    git config --global user.email "rohith@gmail.com"
                    git add deployment.yaml
                    git commit -m 'Updated the deployment file'
                    """

                    // 3. Push the feature branch to the remote repository
                    sh "git push origin feature/argoCdCiCd"

                    // 4. Create a pull request from the feature branch to the target branch
                    def apiUrl = "https://api.github.com/repos/${GIT_USERNAME}/argoCd/pulls"
                    def title = "Update Deployment File"
                    def head = argoCdCiCd
                    def base = "master"

                    sh """
                    curl -X POST \
                        -H 'Authorization: token ${GIT_PASSWORD}' \
                        -d '{
                            "title": "${title}",
                            "head": "${head}",
                            "base": "${base}"
                        }' \
                        ${apiUrl}
                    """
                }
            }
        }
    }
}


}
}
