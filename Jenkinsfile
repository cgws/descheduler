@Library('ciinabox@docker_build') _
pipeline {
  environment {
    REGION = 'ap-southeast-2'
    ECR_REPO = '362995399210.dkr.ecr.ap-southeast-2.amazonaws.com'
    OPS_ACCOUNT_ID = '362995399210'
  }
  agent {
    node {
      label 'docker'
    }
  }
  stages {
    stage('Build docker image') {
      steps {
        script {
          withCredentials([
            [
              $class: 'UsernamePasswordMultiBinding',
              credentialsId: 'catch-github-token',
              usernameVariable: 'GITHUB_USER',
              passwordVariable: 'GITHUB_TOKEN'
            ],
          ]) {
            dockerBuild repo: env.ECR_REPO,
              image: 'catch/descheduler',
              dockerfile: 'Dockerfile',
              tags: [
                env.BRANCH_NAME,
                env.GIT_COMMIT,
                "latest"
              ],
              push: true,
              cleanup: true
          }
        }
      }
    }
  }
}
