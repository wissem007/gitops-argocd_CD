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
        stage("Print BUILD_NUMBER") {
             steps {
                script {
                    //sed -i "s#${APP_NAME}.*#${APP_NAME}:${IMAGE_TAG}#g" deployment.yml
                     echo "${DOCKERTAG}"
                }
        }
        }
        stage("Update kubernates deploy file"){

                    steps{
                        script {
                            sh '''cat dev/deployment.yml
sed -i 's+wissem007/gitops-argocd_ci.*+wissem007/gitops-argocd_ci:'${DOCKERTAG}'+g' dev/deployment.yml
cat dev/deployment.yml
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
git commit -m \'Done by Jenkins Job changemanifest: '${DOCKERTAG}'\'

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