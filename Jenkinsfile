pipeline {
agent any


environment {
    REPO_URL = 'https://github.com/ajaysp17/nginx-test.git'
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

            sed -i 's/nginx:latest/nginx:1.25/g' deployment.yaml
            '''
        }
    }

    stage('Commit & Push') {
        steps {
            sh '''
            git config user.name "jenkins"
            git config user.email "jenkins@company.com"

            git add .
            git commit -m "GitOps: update nginx image" || echo "No changes"

            git push origin main
            '''
        }
    }
}


}

