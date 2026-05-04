pipeline {
agent any


environment {
    REPO_URL = 'git@github.com:ajaysp17/nginx-test.git'
    BRANCH = 'main'
}

stages {

    stage('Checkout') {
        steps {
            git branch: "${BRANCH}", credentialsId: 'git-creds', url: "${REPO_URL}"
        }
    }

    stage('Update Manifest') {
        steps {
            sh '''
            echo "Updating deployment to trigger GitOps sync"
            // echo "Current Commit: ${env.GIT_COMMIT}"
            sed -i 's/nginx:${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}/nginx:${env.GIT_COMMIT}/g' deployment.yaml
            '''
        }
    }

    stage('Commit & Push') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'git-creds', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                sh '''
                git config user.name "jenkins"
                git config user.email "jenkins@company.com"

                git add .
                git commit -m "GitOps: update nginx image" || echo "No changes"

                git remote set-url origin https://${GIT_USER}:${GIT_PASS}@github.com/ajaysp17/nginx-test.git

                git push origin main
                '''
            }
        }
    }
}


}

