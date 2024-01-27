pipeline {
agent any
  stages {
        stage('Git Checkout') {
            steps {
		    //git 'https://github.com/rohith-marigowda/argoCd.git'
		    sh 'git clone https://github.com/rohith-marigowda/argoCd.git'
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
                    git config user.email "abhishek.xyz@gmail.com"
                    git config user.name "Abhishek Veeramalla"
                    BUILD_NUMBER=${BUILD_NUMBER}
                    sed -i 's+rohithmarigowda/assignment.*+rohithmarigowda/assignment:${DOCKERTAG}+g' deployment.yaml
                    git add .
                    git commit -m "Update deployment image to version ${BUILD_NUMBER}"
                    git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                '''
            }
        }
}

	 
  }
}
