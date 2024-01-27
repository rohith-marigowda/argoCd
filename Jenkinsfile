pipeline {
agent any
  stages {
        stage('Checkout SCM'){
            steps {
                git credentialsId: 'github', 
                url: 'https://github.com/rohith-marigowda/argoCd.git',
                branch: 'master'
            }
        }


	  stage('Updating Kubernetes deployment file'){
            steps {
		sh    pwd
                sh "cat deployment.yml"
                sh "sed -i 's/rohithmarigowda/assignment.*/rohithmarigowda/assignment:${DOCKERTAG}/g' deployment.yml"
                sh "cat deployment.yml"
            }
        }
	  
   stage('Update Deployment File') {
	   steps{
		script{
                    sh """
                    git config --global user.name "Rohith"
                    git config --global user.email "rohith@gmail.com"
                    git add deployment.yml
                    git commit -m 'Updated the deployment file' """
                    withCredentials([usernamePassword(credentialsId: 'githubcred', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        sh "git push http://$user:$pass@github.com/rohith-marigowda/argoCd.git master"
                    }
                }
	   }
   }
  }
}
