pipeline {
    agent any

    
    stages {
        stage('Git pull') {
            steps {
                warnError('Git pull') {
                git branch: 'main', credentialsId: '78e4dc85-554e-4c80-9c51-16e7b03ab826', url: 'git@github.com:rangushiva1/app-deploy.git'
                sh "git fetch"

                    }
                }
            }
        stage('Docker push') {
            steps {
                warnError('Dockerfile push') {
                
                sh '''
                whoami
                docker build -f Dockerfile -t demo-app-shiva .
                docker tag demo-app-shiva:latest public.ecr.aws/m2x6m9z0/demo-app-shiva:latest
                docker push public.ecr.aws/m2x6m9z0/demo-app-shiva:latest
                '''

                    }
                }    
        }
        stage('Deploy to EKS') {
            steps {
                warnError('Deploy to EKS') {
                dir('/var/lib/jenkins/workspace/k8s-deploy') {
                git branch: 'main', credentialsId: '78e4dc85-554e-4c80-9c51-16e7b03ab826', url: 'git@github.com:rangushiva1/k8s-files.git'    
                sh '''
                cd /var/lib/jenkins/workspace/k8s-deploy/
                kubectl apply -f deploy-app.yml
                kubectl apply -f loadbal.yml
                '''
                    }
                }
            }
        }
    }
}
