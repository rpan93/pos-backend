pipeline {
    agent any
    environment {
        CI = 'true'
    }
    triggers { pollSCM('H/5 * * * 1-5') }
    
stages {
   stage('Verify changes') {
            steps {
                sh "git log -1 | grep Author | awk '{print \$2}' > lastCommitAuthor.txt "
                script {
                    lastCommitAuthor = readFile("lastCommitAuthor.txt").trim()
                    if ( lastCommitAuthor == "JBackend" ) {
                        echo "There are no changes since the last release. Aborting"
                        currentBuild.result = 'ABORTED'
                        error('Stopping early')
                    } else {
                        echo "Changes detected since last release. Last commit made by ${lastCommitAuthor}. Proceeding."
                    }
                }
            }
        }
  stage('build') {
      steps {
        sh "./gradlew clean build"
        echo "$CI"
      }
  } 
  stage('Run Tests') {
              steps { 
                sh "./gradlew check"
    
              }
  
  }
  stage('deploy-Stage-Server') {
      steps {
       echo 'deploy stage'
      }
  } 
  
} 
 
}
