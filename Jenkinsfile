node {
  properties([
    parameters([
      string(name: 'git_branch_tag_or_commit', defaultValue: '*/master')
    ])
  ])

  stage('Clean'){
    deleteDir()
  }
  stage('Checkout Codedeploy Branch') {
    checkout([
      $class: 'GitSCM',
      branches: [[name: '${git_branch_tag_or_commit}']],
      doGenerateSubmoduleConfigurations: false,
      extensions: [],
      submoduleCfg: [],
      userRemoteConfigs: [[
        credentialsId: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx',
        url: 'git@github.com:rajtupakula/testpipeline.git'
      ]]
    ])
  }
  stages {
        stage('Build') {
            steps {
                sh 'make'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

 stages {
        stage('Test') {
            steps {
                sh 'make check'
            }
        }
    }
    post {
        always {
            junit '**/target/*.xml'
        }
        failure {
            mail to: team@example.com, subject: 'The Pipeline failed :('
        }
    }

}
