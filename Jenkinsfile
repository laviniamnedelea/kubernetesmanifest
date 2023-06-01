node {
    stage('Clone repository') {
        checkout scm
    }

    stage('Update GIT') {
    script {
      catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
        withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
          //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
          sh 'git config user.email lavinia.mndl@gmail.com'
          sh 'git config user.name laviniamnedelea'
          sh "sed -i 's+lavned/masters.*+lavned/masters:${DOCKERTAG}+g' deployment.yaml" //Could be done better, with no hardcoded elements.
          sh 'git add .'
          sh "git commit -m 'Jenkins Job updated the manifest file with version: ${env.BUILD_NUMBER}'" //TODO: will have to update the other images a well.
          sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
        }
      }
    }
    }
}
