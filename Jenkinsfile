pipeline {

  agent {
    node {
      label 'master'
    }
  }

  options {
    timestamps()
  }

  stages {
   
  
  
stage("Create new tag") {
         when {
               expression {env.BRANCH_NAME == 'master'}
            }                     
            steps {
              
              sshagent (credentials: ['44d9ae35-ccce-4c7c-8b27-0580fc5a7379'])                        
                {
                script {
                        sh "git config --add remote.origin.fetch +refs/heads/master:refs/remotes/origin/master"
                        sh "git fetch"
                        def tag = sh(returnStdout: true, script: "git tag | tail -1").trim()
                        println tag
                        def semVerLib = load 'SemVer.groovy'
                        def version = semVerLib.getTagversion(tag)
                        println version
                        sh """
                            git tag -a "v${version}" \
                                -m "Generated by: ${env.JENKINS_URL}" \
                                -m "Job: ${env.JOB_NAME}" \
                                -m "Build: ${env.BUILD_NUMBER}"
                            git push --tags
                        """
                    
              }
                
            }
        }
}

  }

}
