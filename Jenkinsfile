pipeline {
agent any
  stages {
        stage('Git Checkout') {
            steps {
		    //git 'https://github.com/rohith-marigowda/argoCd.git'
		    git branch: 'master', url: 'https://github.com/rohith-marigowda/argoCd.git'
            }
        }
    
   stage('Update Deployment File') {
        environment {
            GIT_REPO_NAME = "argoCd"
            GIT_USER_NAME = "rohith-marigowda"
        }
	 steps {
            withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                sh '''
                    git config user.email "rohith.marigowda@gmail.com"
                    //git config user.name "Rohith"
                    BUILD_NUMBER=${BUILD_NUMBER}
		    echo "before executing sed command"
                    sed -i 's+rohithmarigowda/assignment.*+rohithmarigowda/assignment:${DOCKERTAG}+g' deployment.yaml
		    echo "sed command is executed"
                    git add .
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
}

	 
  }
}
