pipeline {
    agent {
         node { label 'jenkins-test' } 
    } 

    stages {
        stage('Build') {
            steps {
                script {
                        sh """ 
                            echo 'Building..'
                        """
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh """ 
                        echo 'Testing..'
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh """ 
                        echo 'Deploying....'
                    """
                }
            }
        }
    }

     post { 
        always { 
            echo 'I will always say Hello again!'
        }
    }
    
}