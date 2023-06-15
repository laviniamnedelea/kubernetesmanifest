pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Update GIT') {
      steps {
        script {
          catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
            withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME'),
            usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')
            ]) {
              sh "git config user.name ${GIT_USERNAME}"
              sh "sed -i 's|${DOCKERHUB_USERNAME}/masters.*|${DOCKERHUB_USERNAME}/masters:${DOCKERTAG}|g' deployment.yaml" 
              sh "sed -i 's|${DOCKERHUB_USERNAME}/masters.*|${DOCKERHUB_USERNAME}/masters:${DOCKERTAG}|g' deployment-fe.yaml"
              sh 'git add .'
              sh "git commit -m 'Jenkins Job updated the manifest files for fe and be with version: ${env.BUILD_NUMBER}'"
              sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/kubernetesmanifest.git HEAD:main"
            }
          }
        }
      }
    }

    // stage('Update GIT') {
    //   steps {
    //     deployWithManifest([
    //       manifestfile: "${params.manifest}" // parse manifest
    //   ])
    //   }
    // }
  }
}
