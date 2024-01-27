pipeline {
agent any
  stages {
        stage('Git Checkout') {
            steps {
		    //git 'https://github.com/rohith-marigowda/argoCd.git'
		    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/rohith-marigowda/argoCd'
            }
        }
    
  stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email rohith.marigowda@gmail.com"
                        sh "git config user.name Rohith Gowda"
                        //sh "git switch master"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+rohithmarigowda/assignment.*+rohithmarigowda/assignment:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
      }
    }
  }
}
    
}
