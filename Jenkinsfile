pipeline{
    agent any

    stages{
        stage("clenup workspace"){
            steps{
                   script {
                    // sh "rm -rf *"
                    cleanWs()

                }
            }
 
        }
        stage("checkout  SCM"){
                    steps{
                        script {
                            git credentialsId: 'github',
                            url: 'https://github.com/wissem007/gitops-argocd_CD.git',
                            branch: 'master'

                        }
                    
                    }
                    
                }
                    stage("Update kubernates deploy file"){
                    steps{
                        script {
                            sh '''cat deployment.yml
sed -i "s#${APP_NAME}.*#${APP_NAME}:${IMAGE_TAG}#g" deployment.yml
cat deployment.yml
'''
                         }
                    
                    }
            }
                    stage("Update GIT"){
                    steps{
                        script {
                            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {

                   
                        //withCredentials([string(credentialsId: 'github', variable: 'GIT_TOKEN')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh '''git config --global user.email "alouiwiss@gmail.com"
git config --global user.name "wissem007" 
git add deployment.yml
git commit -m \'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}\'

'''
      withCredentials([gitUsernamePassword(credentialsId: 'jenkins_github', gitToolName: 'Default')]) {
       sh 'git push https://github.com/wissem007/gitops-argocd_CD.git HEAD:master'

                         }
                    
                    }
            }

    }
 
}

}
 
}