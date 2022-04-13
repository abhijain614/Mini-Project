pipeline {
		agent any 
    stages {
        stage('Git Pull') {
            steps {
				git url: 'https://github.com/abhijain614/Mini-Project.git',
				branch: 'main',
                credentialsId: 'github'
            }
        }
        stage('Maven Build and Test') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Docker build image') {
            steps {
                sh 'docker build -t abhijain614/miniproject:latest .'
            }
        }
        stage('Publish Docker Images') {
            steps {
                withDockerRegistry([ credentialsId: "mini-project", url: "" ]) {
                    sh 'docker push abhijain614/miniproject:latest'
                }
            }
        }
        
        stage('Ansible Deploy') {
             steps {
                  ansiblePlaybook becomeUser: null, colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'playbook.yml' ,sudoUser: null
             }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}
